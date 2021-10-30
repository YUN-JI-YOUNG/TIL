## DOM 조작   

- 선택(Select) -> 변경(Manipulation) 순서로 조작    

### 선택 관련 메서드    

  - 단일 element 반환    

    `Document.querySelector(selector)`   

    - 제공한 선택자와 일치하는 첫번째 element 객체 반환 (없다면 null)     
    - id, class, tag 선택자 등을 모두 사용 가능하여 더 구체적이고 유연하게 선택 가능   

    `getElementById(id)`

  - HTMLCollection 반환   

    `getElementsByTagName(name)` 

    `getElementsByClassName(names)`  

  - NodeList 반환   

    `Document.querySelectorAll(selector)`  

    - 하나 이상의 셀렉터를 포함하는 유효한 CSS selector 을 인자로 받음   
    - 지정된 셀렉터에 일치하는 여러 element를 선택하여 NodeList 반환    
    - id, class, tag 선택자 등을 모두 사용 가능하여 더 구체적이고 유연하게 선택 가능    

- HTMLCollection & NodeList   

  - 배열과 같이 각 항목에 접근하기 위한 index 제공     
  - Live Collection으로 DOM의 변경사항을 실시간 반영    
  - **HTMLCollection**    
    - name, id, index 속성으로 각 항목에 접근 가능  
  - **NodeList **  
    - index로만 각 항목 접근 가능   
    - HTMLCollection과 달리 배열에서 사용하는 forEach 함수 등 다양한 메서드 사용 가능   
    - querySelectorAll() 에 의해 반환되는 NodeList는 Static Collection으로 실시간 반영 X  

- Collection  

  - Live Collection 

    - 문서가 바뀔때마다 DOM의 변경사항을 실시간으로 collection에 반영      

    - 만약 length를 범위로 해서 for 문을 돌고있다면, 변경사항이 생기는 즉시 length도 하나 줄어들어서 원하는대로 출력되지 않을 수도 있음   

      -> 실습코드 99_collection 참고    

  - Static Collection    

    - DOM이 변경되어도 collection 내용에 영향 X    
    - querySelectorAll()의 반환 NodeList만 static Collection    



### 변경 관련 메서드   

  - Creation

    `Document.createElement()`  

    - 작성한 태그명의 HTML 요소를 생성하여 반환  

      ```python
      const ulTag = document.querySelector('ul')
      const newLiTag = document.createElement('li')
      
      # ul 태그 선택 후. li태그 생성 -> 이후 append 하여 ul 태그 밑에 추가
      ```

  - append DOM  

    `Element.append()` 

    - 특정 부모의 Node 자식 NodeList 중 마지막 다음에 Node 객체나 DOMString 삽입   

    - 여러 개의 Node 객체, DOMString  추가 가능   

    - return 값 X   

      ```python
      # const ulTag = document.querySelector('ul')
      # const newLiTag = document.createElement('li')
      
      newLiTag.innerText = '새로운 리스트 태그'
      ulTag.append(newLiTag)
      ulTag.append('문자열도 추가 가능')
      
      const new1 = document.createElement('li')
      new1.innerText = '리스트 1'
      const new2 = document.createElement('li')
      new2.innerText = '리스트 2'
      const new3 = document.createElement('li')
      new3.innerText = '리스트 3'
      ulTag.append(new1, new2, new3)
      ```

    `Node.appendChild()`   

    - 특정 부모의 Node 자식 NodeList 중 마지막 다음에 Node 객체 삽입     

    - 한 번에 하나의 Node만 추가 가능   

    - 주어진 Node가 이미 문서에 존재하는 다른 Node를 참조한다면 새로운 위치로 이동 

    - 추가된 Node 객체 return       

      ```python
      # const ulTag = document.querySelector('ul')
      # const newLiTag = document.createElement('li')
      
      newLiTag.innerText = '새로운 리스트 태그'
      ulTag.appendChild(newLiTag)
      
      # ulTag.appendChild('문자열은 추가 불가') -> 에러 발생
      
      const new1 = document.createElement('li')
      new1.innerText = '리스트 1'
      const new2 = document.createElement('li')
      new2.innerText = '리스트 2'
      ulTag.appendChild(new1, new2)
      # -> 이렇게 여러개 추가하려하면 리스트1만 추가됨  
      ```

### 변경 관련 속성   

  - `Node.innerText`   

    - Node 객체와 그 자손의 텍스트 컨텐츠(DOMString) 표현   

      - `ex>` `<li> </li>` 사이의 텍스트 추가       

    - 해당 요소 내부의 raw text이며 사람이 읽을 수 있는 요소만 남김    

    - 줄바꿈을 인식하고 숨겨진 내용을 무시하는 등 최종적으로 스타일링이 적용된 모습 표현   

      ```python
      liTag1.innerText = '<li>춘천</li>'
      
      # <li>춘천</li> 가 출력 -> li 태그 인식 X
      ```

  - `Element.innerHTML`  

    - 요소 내에 포함된 HTML 마크업 반환   

      ```python
      liTag2.innerHTML = '<li>춘천</li>'
      
      # 춘천  이 출력 -> li 태그 인식 O 
      ```

    - XSS (Cross-site Scripting) 공격에 취약하므로 주의하여 사용    

      - innerHTML은 내부에서 자바스크립트 코드가 실행될 수 있는데 이를 악용하여 

        게시판이나 메일 등에 악성 자바스크립트 코드를 삽입해 개인정보 탈취 가능    

      ```python
      ulTag.innerHTML = 
      '<li><a href="javascript:alert(\'당신의 개인정보 유출\')">춘천</a></li>'
      ```

      

### 삭제 관련 메서드  

  - `ChildNode.remove()`  

    - Node가 속한 트리에서 해당 Node 제거    

    - 삭제할 자식 Node가 호출   

      ```python
      const header = document.querySelector('#location-header')
      header.remove()		# 리턴값 X 
      ```

  - `Node.removeChild()` 

    - 자식 Node를 제거하고 제거된 Node return   

    - 삭제할 자식 Node의 부모 Node가 호출   

      ```python
      const parent = document.querySelector('ul')
      const child = document.querySelector('ul > li')
      const removedChild = parent.removeChild(child)	# 리턴값 존재  
      console.log(removedChild)
      ```



### 속성 관련 메서드  

  - `Element.setAttribute(name, value)`  

    - 지정된 요소의 값 설정   
    - name = class, value = 'ssafy' 라고 설정한다면, 선택한 요소의 class가 ssafy로 변경   
    - 속성이 이미 존재하면 값 갱신, 존재하지 않으면 지정된 이름과 값으로 새 속성 추가   

  - `Element.getAttribute(attributeName)`  

    - 해당 요소의 지정된 값(문자열) 반환   

    - 인자는 값을 얻고자하는 속성의 이름    

      ```python
      const getAttr = document.querySelector('.ssafy-location')
      getAttr.getAttribute('class')	# class 속성 값 return
      getAttr.getAttribute('style')	# style 속성 값 return 
      ```

      - 속성이 없으면 null 반환   

