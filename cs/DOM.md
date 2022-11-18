# DOM
DOM(Document Object Model)은 HTML, XML 문서의 프로그래밍 interface이다.  
기본적으로 여러 프로그램들이 페이지의 콘텐츠 및 구조, 그리고 스타일을 읽고 조작할 수 있도록 API를 제공한다.
DOM은 nodes와 objects로 문서를 표현한다. 이들은 웹 페이지를 스크립트 또는 프로그래밍 언어들에서 사용될 수 있게 연결시켜주는 역할을 담당한다.

웹 페이지는 일종의 문서로서 웹 브라우저를 통해 그 내용이 해석되어 웹 브라우저 화면에 나타나거나 html 소스 자체로 나타나기도 한다.

## 웹 페이지 동작 방식
1. 브라우저에서 읽어들인 문서를 파싱하여 최종적으로 어떤 내용을 렌더링 할지 결정
2. 브라우저에서 렌더링을 수행

![웹 페이지 동작 방식](https://wit.nts-corp.com/wp-content/uploads/2019/02/-1)
첫 번째 과정을 거치면서 렌더링 트리가 생성
렌더 트리는 웹 페이지에 표시될 HTML 요소들과 이와 관련된 스타일 요소들로 구성됩니다. 브라우저는 렌더 트리를 생성하기 위해 다음과 같이 두 모델이 필요합니다.

- DOM(Document Object Model) – HTML 요소들의 구조화된 표현
- CSSOM(Cascading Style Sheets Object Model) – 요소들과 연관된 스타일 정보의 구조화된 표현

## DOM은 어떻게 생성될까?
DOM은 원본 HTML 문서의 객체 기반 표현 방식이다.  
DOM이 갖고 있는 근본적인 차이는 단순 텍스트로 구성된 HTML 문서의 내용과 구조가 객체 모델로 변환되어 다양한 프로그램에서 사용될 수 있다는 점이다.  
DOM의 개체 구조는 “노드 트리”로 표현된다. 하나의 부모 줄기가 여러 개의 자식 나뭇가지를 갖고 있고, 또 각각의 나뭇가지는 잎들을 가질 수 있는 나무와 같은 구조로 이루어져 있기 때문이다.


## Reference
- [Mnd web docs - DOM소개](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/Introduction)

- [DOM은 정확히 무엇일까](https://wit.nts-corp.com/2019/02/14/5522)