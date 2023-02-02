# UIBasics

## git clone 하는 법


이 때, 디렉토리는 바탕화면으로 해두지 않고, program files 쪽으로 잡아놓는다.

디렉토리를 설정한 후, git을 설치하고, cmd를 켜서 'git'을 입력하여 git이 잘 설치되었는지 확인한다.

git 설치 후에도 cmd 창에서 git이 활성화되지 않는다면 재부팅 후 재시도한다.


github에 있는 프로젝트를 fork 해오고 난 후, cmd 창에서

'git clone (dir)'

을 입력한다. (여기서 dir은 디렉토리)


## git과 visual studio 연동


visual studio 오른쪽 아래 바에 '소스 제어에 추가' 메뉴가 있다. 'Git(G)'를 선택해서 Git 서비스에 연동한다.



## 변경 사항 동기화 하기


변동 사항이 있을 때에는 오른쪽 하단 바에 연필 그림이 나타난다.

연필 그림을 클릭하고 뜨는 입력창에 커밋 메세지를 입릭한다.

그 아래에 있는 '모두 커밋'을 클릭하고 푸시한다.

※푸시 하기 전에는 저장한다.





## MVVM

MVVM 패턴은 Model + View + ViewModel 을 합친 용어이다. 


Model: 어플리케이션에서 사용되는 데이터와 그 데이터를 처리하는 부분

View: 사용자에서 보여지는 UI 부분, 디자인, xaml로 작성한다.

ViewModel: View를 표현하기 위해 만든 View를 위한 Model이다. View를 나타내 주기 위한 Model이자 View를 나타내기 위한 데이터를 처리하는 부분이자, 디자인에 연결시킬 데이터 및 데이터의 집합, cs로 작성한다.


## Nodify


examples는 대략적으로 실행 되는 app, MVVM 패턴으로 작성되었다.

Calculator에서 ViewModel이라고 작성된 부분은 데이터를 제공한다.

View.xmal은 View 역할을 해주는 부분, 특별한 경우가 아니면 cs비하인드 코드가 없음, 자동으로 작성된 코드만 있다.

프로그램의 시작 위치는 MainWindow.xaml에서 시작된다.


Nodify 클래스는 실제로 Editor가 있는 부분, 빌드 시 실행 app과 연결될 때에만 동작한다.

여기에는 MainWindow.xaml 이라는 파일은 없다. 이는 Nodify 클래스가 api의 집합체이기 때문에 app에서 entrypoint가 되는 부분이 필요하지 않기 때문이다.


### Themes 파일

실제로 UI를 담당하는 부분이다. 사용자 지정 컨트롤을 만들어주면 자동으로 생성되는데, 이는 vs로 wpf 프로젝트를 만들어보고 그 안에서 사용자 지정 컨트롤을 만들어주면 이해가 쉽다.

Tester 라는 Control에서 상속받은 객체를 만들면, Themes 라는 폴더가 생성되고 Generic.xaml 파일이 생성된다. 이는 ResourceDictionary 이다. 이 안에서 UI를 제작할 수 있다.


Tester 객체와 Generic 과 연결시켜주는 코드:


```
static Tester()
        {
            DefaultStyleKeyProperty.OverrideMetadata(typeof(Tester), new FrameworkPropertyMetadata(typeof(Tester)));
        }
        
```


static constructor를 만들어서 메모리에 올려놓는다.



## Connection.xaml

Connection, LineConnection, CircuitConnection 을 타겟으로 하는 스타일 등이 정의 되어있다.

Connection.xaml은 Connections 폴더 안에 있다.


Controls.xaml은 MergedDictionaries 이다. (여러 ResourceDictionary를 합친 것, 그것을 Generic.xaml로 연결시켜준다.)

Generic.xaml은 기본 리소스를 지정해주는 파일이다. 리소스는 일종의 UI를 정의하는 xaml이고, 특정 UI를 설정해주기도 한다.

Connections 라는 폴더에 실질적으로 개발자가 만들어주는 control들이 나온다.


BaseConnection은 Shape 이라는 컨트롤을 부모로 두는 컨트롤이다.

LineConnection은 BaseConnection에서 상속받았다.

Circuti Connection은 LineConnection에 상속받았다.

Connector는 Control에서 상속받았다.

PendingConnection은 ContentControl에서 상속받았다.

이러한 데에는 다 이유가 있다.


BaseConnection 안에 Shape가 있다.

Shape는 FrameworkElement에서 상속받는 경우와 Control에서 상속받는 경우가 있다.

BaseConnection은 일종의 그려서 나타내는 컨트롤을 나타낸다. drawing api로 그려서 나타내는 control이다.

control을 상속받은 부분은 control 개체를 기반으로 하는 컨트롤이다. ContentControl은 control에서 상속받았다.


```
<Style TargetType="{x:Type local:Connection}">
        <Setter Property="StrokeThickness"
                Value="3" />
        <Setter Property="Stroke"
                Value="DodgerBlue" />
        <Setter Property="Fill"
                Value="DodgerBlue" />
        <Setter Property="Spacing"
                Value="20" />
    </Style>
```

이 코드는 스타일을 지정하는 스타일, 일종의 컨트롤 스타일이다.

Css 와 비슷하다는 지점에서 avalonia 적용 시 이해하는 데에 더 도움이 된다.

Style 안에는 Setter 로 각각의 속성의 값을 적용해준다.

예를 들어 StrokeThickness 라는 DependencyProperty가 있는데 이는 선의 두께를 3으로 했다는 것이다.


## Property&DependencyProperty

BaseConnection.cs 로 들어가보면,

 public static readonly DependencyProperty SourceProperty = DependencyProperty.Register(nameof(Source), typeof(Point), typeof(BaseConnection), new FrameworkPropertyMetadata(BoxValue.Point, FrameworkPropertyMetadataOptions.AffectsRender));

와 같이 되어 있다.

이 Dependency Property는 일반적인 Property와 차이가 있다.

wpf에서 간단한 버튼 하나를 만들고 디자인에서 열어보면, Content 에서 Button 이라고 작성되어 있다. 이 Content에서 Button이라고 작성되어 있다. 여기서 이 Content 가 Dependency Property 이다.


### Dependency Property에 대한 더 세밀한 후술:

Dependency Property는 의존 프로퍼티 라 해석되며, 프로퍼티 값이 변경되었을 때 자동으로 어떤 일을 처리할 수 있게 해주는 것이다. 스타일링, 데이터 바인딩, 에니메이션 등 WPF 주요 부분에 사용된다.

 이것이 사용되는 이유는 각 요소를 사용하는 시점에 Property 값이 결정되고, Static 변수를 사용함으로써 메모리 절약에 효과적이다. 또한 Property를 구현할 때에 필요한 많은 코드를 표준화할 수 있다.

 Dependency Property를 등록할 때에는 DependencyProperty.Register 라는 함수의 파라미터로 Property name, Property type, 또 Dependency Property를 사용하는 class 타입이 꼭 필요하다.
 오버로드 메소드가 필요할 때에는 Metadata를 추가하면 된다.


### 일반 Property와 차이점

 Dependency Property는 값이 변경되었을 때 자동으로 알림을 주는 '변경 통보' 기능이 있다. 일반적인 플랫폼은 데이터 값이 변경되었을 때 UI도 변경되는 기능이 많은데, 이럴 때 Dependency Property를 사용해서 편리하게 구현할 수 있다.




## MvvmTester.MainWindow.xaml

xaml은 xml 형태이다.

Window 테크가 최상단에 위치한다.

```
using System.Drawing;

namespace MvvmTester
{
	public partial class Main Window : Window
	{
		public MainWindow()
		{
			InitializeComponent();
		}
	}
}
```

cs와 xaml을 연결시켜준다.


## 각종 태그(접두사)의 명칭 정의

```
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"

<Button x:Name = "TestBtn">
 ```

에서 x 접두사를 쓰고 있다.

이는 개별 xaml 네임스페이스를 매핑하기 위해 사용된다. 이러한 선언 간의 관계에서 x: 접두사 매핑은 XAML 언어 정의의 일부일 내장 항목을 지원하며, WPF는 XAML을 언어로 사용하고 XAML에 대한 해당 개체의 어휘를 정의하기 위해 사용한다.

:d 접두사의 경우, WPF 디자이너에서 디자이너 네임스페이스로 사용된다. Visual Studio용 WPF 디자이너와 다른 디자인 환경 간의 XAML 교환을 지원하기 위해 네임스페이스가 미리 지정되어 있을 수 있다. d: 네임스페이스의 경우 디자이너에서 XAML 기반 UI를 왕복하는 동안 디자인 상태를 유지하고, 디자이너에서 런타임 데이터 소스를 사용할 수 있게하는 데에도 사용된다.

![ddb8bb38a4c38eabc82353a908c66285](https://user-images.githubusercontent.com/53402289/215984259-55290a12-5a4a-4e98-97ca-e5992fa2c703.jpg)
