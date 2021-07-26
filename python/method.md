## 메서드   

### 1. 문자열 메서드    
</br>    

`'문자열'.(함수명)(x)`   

1. find(x)

   x의 첫번째 위치 반환, 없으면 -1 반환

2. index(x)

   x의 첫번째 위치 반환, 없으면 ValueError

#### 1-1. 문자열 변환 메서드

1. replace(old, new[ ,count])

   old를 new로 바꿔서 `복사본` 반환, count를 지정하면 해당 개수만큼만 시행

    `wooowoo.replace('o', '!', 2)  -> w!!woo` 처럼 개수만큼만 변환

   -  베커스-나우르 표기법(BNF) : 파이썬상의 구문이 아니고, 문서상의 표기법이며 `[] => 선택적 인자` -> 문서 상 표기법이므로 파이썬에서 코드로 실제 함수 정의할 때 사용하면 안됨

2. strip([chars])

   선행, 후행 문자(chars)가 제거된 문자열의 복사본 반환하며 strip(양쪽 제거), lstrip(왼쪽 제거), rstrip(오른쪽 제거)가 있다. chars의 기본값은 공백

   `'와우!\n'.rstrip() = '와우!'` , `'안녕하세요????'.rstrip('?') = '안녕하세요'`

3. split(sep=None)

   문자열을 특정 단위로 나눠 `리스트`로 반환

   `'a b c'.split() = ['a', 'b', 'c']` 처럼 공백을 구분자로 나누면 리스트로 반환

4. 'separator'.join(iterable)  -- **문자열 메서드이므로 문자열에서만 사용 가능**

   반복가능한(iterable) 컨테이너 요소들을 separator(구분자)로 합쳐 `문자열` 반환

   `'!'.join('ssafy') = 's!s!a!f!y'`

5. capitalize()  : 첫 문자를 대문자, 나머지는 소문자 변환

6. title() :( `'`나 공백) 이후의 첫 단어를 대문자 변환

   `'hI! Everyone, I\'m ssafy' -> 'Hi! Everyone, I\'M Ssafy'`

7. upper() : 대문자 변환

8. lower() : 소문자 변환

9. swapcase() : 대<->소문자 변환

#### 1-2. 문자열 검증 메서드 -- **True, False 여부 판단**

1. isalpha() : 유니코드 상의 문자인지 여부

   `isdecimal()  < isdigit()  <  isnumeric()` 순으로 더 큰 범위

2. isupper() : 대문자 여부

3. islower() : 소문자 여부

4. istitle() : 타이틀 형식 여부  - 타이틀 형식은 위의 변환메서드에서 나옴

</br>    

### 2. 리스트 메서드

- 공통, 가변 시퀀스 연산 모두 구현

- 리스트 추가 메서드

#### 2-1. 값 추가,삭제 

  1. .append(x) 

     - 리스트 끝에 값 추가 

     ` cafe.append('cafe')  -> cafe 리스트 끝에 'cafe' 추가` 

  2. .extend(iterable) 

     - 리스트 끝에 항목 추가

     - 리스트에 값이 아닌 iterable의 항목 추가

     `cafe.extend(['cafe'])  == cafe += ['cafe'] -> cafe 리스트 끝에 'cafe 추가`'

     - 문자열 추가할 시, 문자열의 각 항목 추가 

  `cafe.extend('cafe') -> cafe 리스트 끝에 'c','a','f','e' 추가`

  3. .insert(i,x) 

     - 정해진 위치 i 에 값 x 추가
     - i > len(list) 이면, 리스트 맨 끝에 추가

  4. .remove(x)

     - 값이 x인 첫번째 항목 삭제
     - 없는 걸 삭제하려고 하면 ValueError 발생

  5. .pop(i)

     - 정해진 위치 i에 있는 값 삭제하고 해당 항목 반환

     - i가 지정되지 않으면 마지막 항목 삭제하고 반환

       `x = number.pop() -> print(number) = [1,2], x = 3`  

  6. .clear()

     - 리스트 내의 모든 항목 삭제, 인자 X

  

  #### 2-2. 탐색 및 정렬

  1. .index(x)

     - x 가 처음 나오는 위치 찾아서 해당 index 값 반환, 없는 경우 ValueError 발생

  2. .count(x)

     - x 값의 개수 반환, 없으면 0 반환

  3. .sort()

     - 원본 정렬, print하면 None 반환
     - 내장함수인 sorted와 비교 -> sorted는 복사본 반환

     `result = numbers.sort() -> print(numbers,result) = [1,2,3,5] None`

     `result = sorted(numbers) -> print(numbers, result) = [3,2,5,1] [1,2,3,5]`

  4. .reverse()

     - 원본 순서를 반대로 뒤집기, None 반환, 정렬은 X

  

  #### 2-3. 리스트 복사

    - 리스트를 복사하면 같은 리스트의 주소를 참조하기 때문에, 원본이 바뀌면 복사본도 바뀜

    -> 가변인 항목의 특징

    - 얕은 복사(shallow copy)

      -> 연산하고 나면 다른 주소를 참조한다는 사실 이용

      - slice 연산자 `a= b[:]` 를 활용하여 다른 주소를 참조하기 위해 연산된 결과 복사

      - list() `a=list(b)` 활용하여 다른 주소를 참조하는 연산된 결과 복사

      **만약, 2차원 리스트처럼 리스트 안의 원소가 다른 주소를 참조하는 경우, 얕은 복사라서 그 안은 아직 같은 주소를 참조하므로 같이 변경됨**

    - 깊은 복사(deep copy)

      - copy 모듈을 사용해야 함 `import copy`  `a = copy.deepcopy(b)`
      - 원본 리스트 유지를 위해선 깊은 복사 사용

  #### 2-4. 기타       
  
  - List comprehension

    - 표현식과 제어문을 통해 특정 값을 가진 리스트 생성하는 방법

    `[<표현식> for <변수> in <iterable> if <조건식>]`

    `[if else <표현식> for <변수> in <iterable>]`  

    -> if 하나면 맨 뒤지만 , if else가 된다면 앞으로 넘긴다.

    ````python
    # 1~3의 세제곱식 결과를 담는 리스트
    list_c = []         
    for number in range(1,4):      
        list_c.append(number**3)
    
    # List comprehension 이용
    [number**3 for number in range(1,4)]
    ````

    ```python
    ## 1~3 중 짝수인 값만 담는 리스트
    list_e = []
    for i in range(1,4):
        if i % 2 ==0 :
            list_e.append(i)
            
    # List comprehension 이용
    [i for i in range(1,4) if x % 2 == 0]
    ```

  1.  map(function, iterable)

    - 순회 가능한 데이터 구조(iterable)의 모든 요소에 함수를 적용하고, 결과를 map 객체로 반환
    - map object를 list로 형변환 하여 결과 확인 가능

    ` map(str, numbers) -> number 모두에 str() 적용`

    `n,m = map(int, input().split()) -> n과 m에 int가 저장되어 바로 활용 가능`

  

  2.  filter(function, iterable)
    - 순회 가능한 데이터 구조(iterable)의 모든 요소에 함수 적용하고, 결과가 True인 것만 filter 객체로 반환
    - 역시 list로 형변환하여 결과 확인 가능

  3.  zip(*iterables)
    - 복수의 순회가능 데이터구조를 모아 튜플을 원소로 하는 zip 객체 반환
    - 역시 list로 형변환하여 결과 확인 가능

  </br>    

  ### 3. 세트 메서드   

  변경 가능, 순서 X, 순회 가능

  #### 3-1. 값 추가

  `.add(element)` -> 어차피 순서 없으므로 그냥 값 추가

  `.update(*others)` -> 여러 값 추가, 중복 안되는 값만 추가

  #### 3-2. 값 제거

  `.remove(elemet)` -> 세트에서 삭제, 없으면 KeyError 반환

  `.discard(element)` -> 세트에서 삭제하고, 없어도 아무 에러 발생안하며 그대로 반환

  `.pop()` -> 순서가 없으므로 인데스값 없음, **임의의 원소** 제거하여 반환, 빈 세트는 KeyError

 </br>     

  ### 4. 딕셔너리 메서드    

  변경 가능, 순서 X, 순회 가능

  #### 4-1. 조회

  `.get(key[,default])` -> key에 대응하는 value 반환, key가 딕셔너리에 없으면 KeyError가 아닌 default(= 기본 값은 None)반환

  `.pop(key[,default])` -> key를 제거하고 해당 값 반환, key가 딕셔너리에 없으면 default 반환, 하지만 key가 딕셔너리에 없고 default 값도 없으면 KeyError 발생

  #### 4-2. 추가 및 삭제

  `.update()` -> 기존 key를 덮어쓰며 제공하는 key, value로 갱신 , 키워드 인자 활용

  키워드 인자 활용할 땐, key type이 자동으로 변환되므로 ''등 사용할 필요 X

  `a = ['apple'= '사나']  a.update(apple='사과')` => `print(a) = {'apple' = '사과'}`

  #### 4-3. 순회

    - 기본적으로 key를 순회하며 key를 통해 값 활용, value 반환
    - 추가 메서드를 활용하여 순회 가능
      - key() : key만 추출
      - values() : Value만 추출
      - items() : (key, value)의 튜플로 구성

 #### 4.4 기타    
 - Dictionary comprehension

    `{key : value for <변수> in < iterable if <조건식>}` 
