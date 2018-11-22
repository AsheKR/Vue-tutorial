# Computed && watch

## computed 속성

템플릿 내 표현식을 넣게되면 코드가 비대해지고 유지보수가 어렵게된다.

### 기본 예제

```html
<div id="example">
  <p>{{ message }}</p>
  <p>{{ reversedMessage }}</p>
</div>
```

```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: '안녕하세요'
  },
  computed: {
    reversedMessage: function() {
      return this.message.split('').reverse().join('')
    }
  }
})
```

위의 `reversedMessage`는 파이썬의 `get property`처럼 사용된다.
`vm.message`에 의존적이며 `vm.message`가 변할경우 `reversedMessage`도 변하게된다.

### computed 속성의 캐싱 vs 메소드

computed 내부에 작성된 메서드는 종속된 대상이 변경되지 않으면 캐싱되고 변경될때만 호출이 일어난다.
method 내부에 `property`처럼 작성한다면 항상 호출하여 손해가 발생한다.

### computed vs watch

Vue 인스턴스의 데이터 변경을 관찰하고 이에 반응하는 보다 일반적인 `watch` 속성을 제공한다.

> computed는 종속된 대상의 변경에 따라 갱신되고,
> watch는 목표 데이터의 변경에 따라 갱신된다.

### computed 속성의 setter 함수

computed 속성은 기본적으로 `getter`만 가지고 있지만 필요한경우 setter를 사용할 수 있다.

```javascript
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

#  watch 속성

대부분의 경우 computed 속성이 적합하지만 사용자가 만든 감시자가 필요한 경우가 있다.
데이터변경에 대한 응답으로 비동기식 또는 시간이 많이 소요되는 조작을 수행하려는 경우 유용하다.
