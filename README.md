# UIBasics

git clone 하는 법


이 때, 디렉토리는 바탕화면으로 해두지 않고, program files 쪽으로 잡아놓는다.

디렉토리를 설정한 후, git을 설치하고, cmd를 켜서 'git'을 입력하여 git이 잘 설치되었는지 확인한다.

git 설치 후에도 cmd 창에서 git이 활성화되지 않는다면 재부팅 후 재시도한다.


github에 있는 프로젝트를 fork 해오고 난 후, cmd 창에서

'git clone (dir)'

을 입력한다. (여기서 dir은 디렉토리)


git과 visual studio 연동


visual studio 오른쪽 아래 바에 '소스 제어에 추가' 메뉴가 있다. 'Git(G)'를 선택해서 Git 서비스에 연동한다.



변경 사항 동기화 하기


변동 사항이 있을 때에는 오른쪽 하단 바에 연필 그림이 나타난다.

연필 그림을 클릭하고 뜨는 입력창에 커밋 메세지를 입릭한다.

그 아래에 있는 '모두 커밋'을 클릭하고 푸시한다.

※푸시 하기 전에는 저장한다.





MVVM

MVVM 패턴은 Model + View + ViewModel 을 합친 용어이다. 


Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분

View: 사용자에서 보여지는 UI 부분, 디자인, xaml로 작성한다.

ViewModel: View를 표현하기 위해 만든 View를 위한 Model이다. View를 나타내 주기 위한 Model이자 View를 나타내기 위한 데이터를 처리하는 부분이자,
디자인에 연결시킬 데이터 및 데이터의 집합, cs로 작성한다.
