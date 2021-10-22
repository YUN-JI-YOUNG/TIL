### M : N 관계    

1:N 관계는 완전한 종속 관계이지만 , 

M:N 관계는 의사-> 환자, 환자-> 의사의 두가지 형태로 모두 표현이 가능한 것     

#### Intro  

- 병원 진료 기록 시스템    

  - 환자 -> 의사 : 진료 예약    
  - 의사 -> 환자 : 예약을 받아 기록 데이터 받음    
  - 이 둘의 관계를 표현하는 것을 모델링이라고 하는데, 모델링은 현실 세계를 최대한 유사하게 반영하기 위한 것   
  - DB를 모델링하고 그 내부에서 일어나는 데이터의 흐름을 어떻게 제어할 수 있을지에 대한 고민 필요   

- 1:N의 한계    

  - 의사(1) : 환자(N)으로 설정할 경우 문제       

  - 1. 이미 진료받은 환자가 새로운 예약을 생성하고자 한다면, 새롭게 객체를 생성해야함     

    2. 1명의 환자가 여러 명의 의사에게 진료 받았을 경우 모두 저장하는 것은 불가  

       -> 하나의 외래키에 여러 개의 의사 데이터를 넣을 수 없음    

    

    - 의사 2명, 환자 2명 생성   

    ```python
    # shell_plus
    
    doctor1 = Doctor.objects.create(name='justin')
    doctor2 = Doctor.objects.create(name='eric')
    
    patient1 = Patient.objects.create(name='tony',doctor=doctor1)
    patient2 = Patient.objects.create(name='harry',doctor = doctor2)
    
    ```

    - 1번 환자가 2번 의사에게도 방문하려고 한다면, 새로운 예약 생성 필요    

      ```python
      patient3 = Patient.objects.create(name='tony', doctor = doctor2)
      ```

      - 기존 예약을 유지한 상태로 새로운 예약을 생성하는데, 새로운 객체가 생성된 것이므로 환자 이름은 같지만 1번 환자 tony 와 3번 환자 tony는 서로 다름    

    - 4번 환자가 의사1,의사2 모두에게 예약을 하려고하면 에러 발생    

      ```python
      patient4 = Patient.objects.create(name='harry', doctor=doctor1,doctor2)
      
      -> SyntaxError: positional argument follows keyword argument
      ```

- 이러한 한계를 극복하기 위해 중개 모델 필요   

  - 의사에 대해서도 1:N, 환자에 대해서도 1:N 관계를 갖는 중개모델     
  - 그러므로 중개모델을 의사와 환자에 대해서 각각의 외래 키 보유    

  ```python
  class Doctor(models.Model):
      name = models.TextField()
  
      def __str__(self):
          return f'{self.pk}번 의사 {self.name}'
  
  class Patient(models.Model):
      name = models.TextField()
  
      def __str__(self):
          return f'{self.pk}번 환자 {self.name}'
  
  # 중개모델 작성
  class Reservation(models.Model):
      doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
      patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
  
      def __str__(self):
          return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'
  ```

  - 의사 1명, 환자 1명 생성   

    ```python
    doctor1 = Doctor.objects.create(name='justin')
    patient1 = Patient.objects.create(name='tony')
    ```

  - 예약 생성   

    - 각각의 외래키를 갖고 있으므로 각각 설정   

    ```python
    Reservation.objects.create(doctor=doctor1, patient=patient1)
    ```

  - 진료 확인   

    ```python
    # doctor1의 모든 예약 조회
    doctor1.reservation_set.all()
    
    # patient1의 모든 예약 조회
    patient1.reservation_set.all()
    ```

  - 의사 1에게 예약 추가 후 조회      

    ```python
    patient2 = Patient.objects.create(name='harry')
    Reservation.objects.create(doctor=doctor1, patient=patient2)
    doctor1.reservation_set.all()
    
    # for 문으로 순회해서 환자이름 출력도 가능   
     for reservation in doctor1.reservation_set.all():
       ...:     print(reservation.patient.name)
    ```

     

#### ManyToManyField    

- 다대다 관계 설정 시 사용하는 모델 필드   

- Django에서 1:N 관계에선 ForeignKey Field 를 제공한 것처럼, M:N 관계에선 ManyToManyField 제공  

- 하나의 필수 위치인자(M:N 관계로 설정할 모델 클래스) 필요     

- 중개 테이블 이름은 `앱이름_클래스이름_MTM 필드이름` 으로 생성 

  - `ex>` hospitals_patient_doctors   

    -> Patient 클래스 내부에 MTM 필드 doctors 가 작성된 경우    

- 중개 테이블의 필드 생성 규칙    

  - `source model` != `target model `  
    - <containing_model> id
    - <other_model>_id
  - `source model` == `target model `  (재귀 관계)
    - `from_<model>_id `   
    - `to_<model>_id`     

  

  ```python
  class Doctor(models.Model):
      name = models.TextField()
  
      def __str__(self):
          return f'{self.pk}번 의사 {self.name}'
  
  
  class Patient(models.Model):
      # ManyToManyField 작성
      name = models.TextField()
      doctors = models.ManyToManyField(Doctor)
  
      def __str__(self):
          return f'{self.pk}번 환자 {self.name}'
  ```

  - Doctor 이나 Patient 둘 중 어디에 작성해도 상관 X    

  - 장고에선 중개모델을 직접 작성하지 않아도 doctor - patient 관계 모델 생성 가능    

    - 의사 1명, 환자 2명 생성   

      ```python
      doctor1 = Doctor.objects.create(name='justin')
      patient1 = Patient.objects.create(name='tony')
      patient2 = Patient.objects.create(name='harry')
      ```

    - 예약 생성 및 조회   

      ```python
      # 환자 입장 (참조)
      patient1.doctors.add(doctor1)
      
      patient1.doctors.all()
      
      # 의사 입장 (역참조)  
      doctor1.patient_set.add(patient2)
      
      doctor1.patient_set.all()
      ```

      - ManyToMany 필드는 doctor나 patient 둘 중 어느 곳에 작성해도 상관없다고 하였지만, 지금은 patient쪽에 작성하였으므로 patient1.doctors 사용   
      - 모두 동일하지만 참조, 역참조에 따라 모델 매니저에 차이 존재    

    - 예약 삭제   

      ```python
      # 의사 입장
      doctor1.patient_set.remove(patient1)
          
      # 환자 입장
      patient2.doctors.remove(doctor1)
      ```



- ManyToManyField 인자   

  - related_name   

    - target model이 source model(관계필드 보유)을 참조할 때 사용 할 manager의 이름 설정   

      = 역참조 시 사용하는 manager의 이름 설정 (`ex` : patient_set)      

    - ForeignKey의 realted_name과 완전히 동일   

      ```python
      doctors = models.ManyToManyField(Doctor, related_name='patients')
      ```

      -> `doctor1.patient_set.all()` 대신, `doctor1.patients.all()`     

  - through   

    - 중개 테이블을 직접 작성해야하는 경우, through 옵션을 사용하여 중개테이블을 나타내는 Django 모델 지정 가능    

    - 중개 테이블에 추가 데이터를 사용해 다대다 관계로 연결하려는 경우에 주로 사용    

      - Django의 MTM 필드는 각 외래키 생성까지만 해주므로 추가 데이터를 사용하고 싶은 경우엔 직접 작성하는 수 밖에 없음     

      - 중개모델을 새로 작성하는데 MTM 필드도 필요한 이유   

        1. MTM 필드를 사용하면, 중개모델을 통해서 참조하더라도 직접 참조하는 것 처럼 데이터를 얻을 수 있음

        ```python
        # 중개모델만 사용
        doctor1.reservation_set.all() 
        -> Reservation: 1번 의사의 1번 환자
        # Reservation 에 대한 객체 정보 출려  
        
        # MTM 필드와 중개모델 사용   
        doctor1.patient_set.all()
        -> Patient : 2번 환자 harry 
        # 의사 입장에서 환자 데이터를 직접 참조하는 것과 동일한 효과  
        ```

        2. add나 remove 같은 메서드를 사용할 경우 과정 생략 가능   

        ```python
        # 중개모델만 사용
        Reservation 내부에서 의사1과 환자1에 해당하는 예약을 조회
        -> 그 후 reservation.delete() 
        
        # MTM 필드와 중개모델 사용  
        # 의사입장
        doctor1.patient_set.remove(patient1)
        
        # 환자입장
        patient2.doctors.remove(doctor1)
        ```

        

    ```python
    # 증상과 예약날짜 추가    
    # models.py
    
    class Doctor(models.Model):
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 의사 {self.name}'
    
    class Patient(models.Model):
        doctors = models.ManyToManyField(Doctor, through='Reservation')
        name = models.TextField()
    
        def __str__(self):
            return f'{self.pk}번 환자 {self.name}'
    
    class Reservation(models.Model):
        doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
        patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
        symptom = models.TextField()
        reserved_at = models.DateTimeField(auto_now_add=True)
    
        def __str__(self):
            return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
    ```

    - 예약 생성   

      ```python
      # 의사, 환자 생성
      doctor1 = Doctor.objects.create(name='justin')
      patient1 = Patient.objects.create(name='tony')
      patient2 = Patient.objects.create(name='harry')
      
      # 예약 생성- 예약 입장   
      reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')
      reservation1.save()
      
      # 예약 생성- 환자 입장 (추가데이터만 작성하면 됨)  
      patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
      
      # 예약 조회
      doctor1.patient_set.all()
      patient1.doctors.all()
      ```

    

  - symmetrical   

    - MTM 필드가 동일한 모델을 가리키는 정의에서만 사용   

    - True(기본값)일 경우, 장고는 person_set 매니저 추가 X   

    - source 모델의 인스턴스가 target 모델의 인스턴스를 참조하면, target 모델 인스턴스도 source 모델 인스턴스를 자동으로 참조하도록 함    

      = True이면, 내가 상대방을 follow하면 상대방도 자동으로 맞팔한다는 뜻    

    - 대칭을 원하지 않을 경우 False로 설정    

    - Follow 기능 구현 시 사용    



- RelatedManager    
  - 일대다 또는 다대다 관련 컨텍스트에 사용되는 manager (`ex>` patients)     
  - 모델 필드의 RelatedManager를 사용하여 개체를 추가, 제거, 생성하는 메서드 사용 가능      
    - add(), remove(), create(), clear() ...      
    - add()
      - 지정된 객체를 관련 객체 집합에 추가   
      - 이미 존재하는 관계에 사용하면 관계가 복제되진 않음    
      - 모델 인스턴스, 필드값(pk)를 인자로 허용    
    - remove()  
      - 관련 객체 집합에서 지정된 모델 객체 제거   
      - 내부적으로 QuerySet.delete()를 사용하여 관계 삭제   
      - 모델 인스턴스, 필드값(pk)를 인자로 허용   
  - 같은 이름의 메서드여도 각 관계에 따라 다르게 사용 및 동작     
    - 1:N 에서는 target 모델 인스턴스만 사용 가능    
    - M:N 에서는 관련된 두 객체 모두 사용 가능   

