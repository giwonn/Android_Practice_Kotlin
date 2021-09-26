# Android_Practice_Kotlin
코틀린으로 제작하는 안드로이드 클론코딩 연습

## 개요
안드로이드 폰이 동작하지 않아 Flutter로 갈아탔다가...  
<br>
다시 동작하여 Java로 왔다가...  
<br>
Kotlin으로 갈아타기 위해 클론코딩 연습을 합니다.  
<br>
이제 그만 갈아타고 클론코딩부터 차근차근 끝내자


# 프로젝트 생성 후 해야할 것
## BindingView 설정
### findViewById 와의 차이점
- Null 안전 : View 결합은 View의 직접 참조를 생성하지 않아 View ID로 인해 null 포인터 예외를 걱정할 필요가 없다.
- 유형 안전 : 각 바인딩 클래스에 있는 필드의 유형이 XML 파일에서 참조하는 View와 일치한다. 즉, 클래스 변환 예외 발생 위험이 없다.

### 단점
- **레이아웃 변수** 또는 **레이아웃 표현식**을 지원하지 않아 XML 파일에서 동적 UI 컨텐츠를 선언할 수 없다.
- 양방향 데이터 결합을 지원하지 않는다.

<br>

### 1. build.grade (:app) 에서 buildFeatures 추가
```gradle
android {
    // ...생략...
  
    buildFeatures {
        viewBinding true
    }

    // ...생략...
}
```

### 2-1. Activity의 경우
```kt
// binding이 필요한 시점에 lateinit으로 늦은 초기화를 해줌
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)

    // bindingView를 사용하기 위해 정적 inflate() 메서드 호출
    binding = ActivityMainBinding.inflate(layoutInflater)

    // R.layout.activity_main -> binding.root
    setContentView(binding.root)


    // 이런식으로 불러올 view에 binding을 붙여서 사용하면 됨
    binding.clickButton.setOnClickListener {
        binding.textView.text = "버튼을 눌렀습니다"
    }

}
```

### 2-2. Fragment의 경우
Nullable 처리를 위해 추가적인 코드가 필요하다.
```kt
private var _binding: ResultProfileBinding? = null
// This property is only valid between onCreateView and
// onDestroyView.
private val binding get() = _binding!!

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    _binding = ResultProfileBinding.inflate(inflater, container, false)
    val view = binding.root
    return view
}

override fun onDestroyView() {
    super.onDestroyView()
    _binding = null
}

// 이런식으로 불러올 view에 binding을 붙여서 사용하면 됨
binding.clickButton.setOnClickListener {
    binding.textView.text = "버튼을 눌렀습니다"
}
```




---

### Reference
[https://developer.android.com/topic/libraries/view-binding](https://developer.android.com/topic/libraries/view-binding)
[Android Studio 4.1에서 제거된 Kotlin Android Extensions을 알아보자.](https://thdev.tech/android/2020/10/07/Remove-kotlinx-synthetic/)
