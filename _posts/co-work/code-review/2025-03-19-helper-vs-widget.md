
---
title: Flutter Widget 반환 메서드
categories: [co-work, code-review]
tags: [flutter] # TAG names should always be lowercase
author: roky
---

## Issue 

# Widget을 코드로 표현하기
## 방법
<img src="/public/paste-image/2025-03-19-test/2025-03-19-1.png" width="100%" />

```dart
class MyWidget extends StatelessWidget {
    @override
    Widget build(BuildContext context) {
        return ListView(
            children: <Widget> [
                for(final obj in myObjs)
                    Dismissible(
                        key: ValueKey(obj.id),
                        child: ...
                    )
            ]
        );
    }
}
```

위의 코드를 분리한다고 했을 때, 크게 2가지 방법으로 위젯을 나타낼 수 있을 것입니다.
1. 헬퍼메서드를 이용하기
해당 부분에서 위와 같은 부분을 메서드를 이용해 분리한다면 아래와 같이나타낼 수 있게됩니다.
```dart
Widget _buildWidget(MyObj obj) {
    return Dismissible(
        key: ValueKey(obj.id),
        child: ...
    );
}
```

2. 위젯자체를 분리하기
위젯 자체를 분리하면 아래와 같이 위젯에 생성자를 받아 나타낼 수 있습니다.
```dart
class MyDismissibleCard extends StatelessWidget {
    const MyDismissibleCard({
        Key? key,
        required this.obj,
    }) : super(key: key);
    final MyObj obj;

    @override
    Widget build(BuildContext context) {
        return Dismissible(
            key: ValueKey(obj.id),
            child: ...
        );
    }
}
```

여기까지만 보았을 때에는 헬퍼메서드를 쓰는것이 코드수로 보았을 때나 사용성을 보았을 때 훨씬 더 깔끔한 느낌을 받습니다. 하지만 정말 헬퍼메서드를 사용해서 위젯을 나타내는것이 좋을까요?

### 별개의 위젯을 선호하는 이유들
아쉽게도 우리의 직관(?)과는 다르게 몇가지의 단점들이 존재한다고 합니다.

예를들어 아래와 같은 코드가 있다고 합시다.
```dart
class BigUIElementState extends State<BigUIElement> {
    @override
    Widget build(BuildContext context) {
        return Stack(
            children: <Widget> [
                _expensiveWidget1(),
                _expensiveWidget2(),
                _favoriteIcon(),
            ],
        );
    }
    
    Widget _favoriteIcon() {
        return Position(
            top: 10,
            right: 10,
            child: IconButton(
                icon: ...,
                onPressed: () => 
                    setState(...),
            ),
        )
    }
}
```

위의 해당 코드에서 _favoriteIcon 버튼을 누르면 어떠한 일이 발생할지 상상해봅시다.
해당 코드를 보면 클릭했을 경우 setState를 통해 다음 프레임에서 build메서드가 다시 호출됩니다.
이때 favoriteIcon만 클릭했을 뿐인데, _expensiveWidget1, _expensiveWidget2 위젯이 불필요하게 랜더링되는 현상이 나타나게 됩니다.
해당 위젯이 불변객체거나 용량이 적다면 성능상 큰 차이는 없겠지만, 해당 위젯이 담고있는 크기가 크거나 복잡한 애니메이션과 같은 부분들의 내용을 담고 있다면 원치 않은 상황을 만들어 낼 수도있습니다.

provider, riverpod의 창시자인 Remi Rousselet 또한 아래와 같은 말을 남겼다고 합니다.
"클래스는 더 나은 방식을 갖는다. 메서드의 유일한 이점은 아주 약간 적게 코드를 작성한다는 것인데, 기능적인 이점은 없습니다."

하나의 프로젝트에서 위젯을 나타낼 때에 클래스, 함수 2가지 방법을 모두 이용해서 나타낼 수 있지만 프로젝트 코드의 일관성을 위해 되도록 클래스를 이용하여 위젯을 나타내야겠습니다.


<img src="/public/paste-image/2025-03-19-test/2025-03-19-15-28-51.png" width="100%" />



<img src="/public/paste-image/2025-03-19-test/2025-03-19-15-28-23.png" width="100%" />

