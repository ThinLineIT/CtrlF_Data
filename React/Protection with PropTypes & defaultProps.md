# Typechecking With PropTypes

[PropTypes와 함께 하는 타입 확인](https://ko.reactjs.org/docs/typechecking-with-proptypes.html)

이 포스트는 위 리액트 공식문서를 참고하여 작성하였습니다.



우리는 예제 코드 위에서 코드를 개선하고 수정해나가면서, 우리가 정확한 Props 정보를 받고 있는지에 대해 체크할 수 있는 방법을 알아보겠습니다

즉, 부모 컴포넌트로부터 전달받은 props가 우리가 원했던 props인지를 체크하는 방법에 대해서 알아보겠습니다. 밑 예제 코드에서 rating은 배달의 민족 혹은 쿠팡 이츠에서 음식을 시킬 때 표시되는 식당에 대한 평점과 같은 개념입니다. 따라서 5 이하의 숫자로 랜덤하게 입력했습니다. 

우선 다음 예제 코드를 보면서 시작해보겠습니다.

변수 foodILike는 array입니다. 다음과 같이 foodILike array에는 object들이 들어있습니다.



```javascript
import React from 'react';

const fodeILike = [];

function Food({ name, picture, rating }) {
​	return (
		<div>	
	 		<h2>I like {name}</h2>
			<h4>{rating}/5.0</h4>
			<img src={picture} alt={name} />
		</div>
	);
}

function App() {
    return (
    	<div>
    		{foodILike.map((dish) => (
    			<Food
    				key={dish.id}
					name={dish.name}
					picture={dish.image}
					rating={dish.rating}
				/>
             ))}
        </div>
	);
}



// foodILike

const foodILike = [
  {
    id: 2,
    name: "Samgyeopsal",
    image:
      "https://3.bp.blogspot.com/-hKwIBxIVcQw/WfsewX3fhJI/AAAAAAAAALk/yHxnxFXcfx4ZKSfHS_RQNKjw3bAC03AnACLcBGAs/s400/DSC07624.jpg"
    rating: 4.9
  },
  {
    id: 3,
    name: "Bibimbap",
    image:
      "http://cdn-image.myrecipes.com/sites/default/files/styles/4_3_horizontal_-		  _1200x900/public/image/recipes/ck/12/03/bibimbop-ck-x.jpg?itok=RoXlp6Xb"
    rating: 4.8
  },
  {
    id: 4,
    name: "Doncasu",
    image:
      "https://s3-media3.fl.yelpcdn.com/bphoto/7F9eTTQ_yxaWIRytAu5feA/ls.jpg"
    rating: 5.5
  },
  {
    id: 5,
    name: "Kimbap",
    image:
      "http://cdn2.koreanbapsang.com/wp-content/uploads/2012/05/DSC_1238r-e1454170512295.jpg"
    rating: 4.7
  }
];
```



자 이제 부모 컴포넌트로부터 전달받은 props가 우리가 원했던 props인지를 정확히 체크해주는 기능을 제공하는
prop-types라고 하는 것을 사용해겠습니다.

이를 사용하기 위해서는 다음과 같은 명령어를 입력하여 prop-types를 설치해 줍니다.

```bash
npm i prop-types 
```

설치 완료 후, package.json에 들어가 dependencies에 prop-types가 들어가 있으면 설치가 잘 된것입니다.





자 이제 prop-type를 사용해서 어떻게 체크하는지 알아보겠습니다.



우선 import 해줘야 합니다.

```
import PropTypes from "prop-types";
```



그리고 다음과 같은 코드를 입력해줘야 합니다.

```
Food.proptTypes = {}; 
```

비어있는 객체 상태인데요, 우리는 이제 비어있는 공간에 우리가 얻고 싶은 props에 대한 설명을 적어줄 것입니다. 즉, Food.propTypes에서 props를 체크할 것입니다.



다음과 같이 비어있는 객체 안에 props에 대한 설명을 적음으로써 우리는 props를 체크할 수 있습니다.



```javascript
Food.proptType = {
    name: PropTypes.string.isRequired,
    picture:  PropTypes.string.isRequired,
    rating:  PropTypes.number.isRequired,
};
```



위의 코드는 정상 동작하는 좋은 코드입니다, 

하지만 propTypes에 대한 빠른 이해를 위해 다음과 같이 위 코드의 내용을 수정해보겠습니다.

```
rating: PropTypes.string.isRequired,
```

분명 우리는 rating에 number값을 전달했었습니다. 그런데 만약 위 코드처럼 number type이 아닌, string이라고 적으면 어떻게 될까요? 확인해보겠습니다.

우선 페이지에는 시각적으로 에러는 없습니다. 하지만 우리가 console로 가서 확인하면, prop-type에 뭔가 실패했다고 말하고 있습니다.

![img](https://blog.kakaocdn.net/dn/7iyrF/btq8wzIimzf/ElCpZ9Q0LyBTEQ37ohub70/img.png)

"rating의 type은 숫자로 제공됐지만 우리는 string을 기대하고 있다." 라고 에러가 나와있네요.

자 보다시피 propTypes는 매우매우 유용합니다. 따라서 여기서 저희가 썼듯이, rating을 string으로 기대하고 있다고 하지만 사실 여기서 우리가 제공한 이 dish.rating은 string이 아니라 number입니다. 따라서 다시 코드를 수정해서 string 대신에 옳은 type인 number를 넣어주도록 하겠습니다.

```
rating: PropTypes.number.isRequired,
```

그리고 확인해보면, 좋습니다. Warning이 없습니다 ! 



이렇게 PropTypes는 우리에게 매우 유용한 tool로 사용되어질 수 있습니다. 또한 위 예제 코드처럼 이런 string 또는 number같은 예제 뿐 아니라 array인지, boolean인지 true인지 false인지 object인지 있는지 없는지에 대해서도 체크할 수 있습니다. 사실상 더 나아가, 우리는 propTypes를 이용하여 우리가 원하는 거의 모든 걸 체크하여 사용할 수 있습니다.  

또한 isRequired는 꼭 써야 하는 것만은 필수요소는 아닌 것을 기억할 필요가 있습니다. isRequired는 propTypes의 부가적인 기능으로, 때때로 구성 요소가 필수적으로 정의해야하거나 제공해야하는 속성을 정의할 수 있습니다. isRequired를 빼주면 우린 type에 대해서만 체크할 수 있고, 반대로 isRequired를 호출하여서 type과 required에 대해서 체크할 수 있습니다. 



예를 들어서 rating에 isRequired를 호출하지 않고 refresh해보겠습니다. 어떻게 동작할까요?



아무런 문제가 없이 잘 동작합니다! console을 봐도 에러는 없습니다. 왜냐면 Food의 rating이 number여야 한다고는했지만 필수는 아니라고 했기 때문입니다. 이 말은 number 또는 undefined라는 말입니다. 따라서 이 경우는 에러가 되지 않습니다. 에러는 오히려 rating을 string으로 변경하면 생기겠죠. 

그렇다면 이렇게 해보는 것은 어떨까요? 다시 rating에 isRequired를 호출하는 대신, kimchi에서 rating을 삭제하고 실행해보겠습니다. 

```javascript
const foodILike = [
  {
    id: 1,
    name: 'Kimchi',
    image:
      'https://www.kgnews.co.kr/data/photos/20201249/art_16068076531596_c04e9e.jpg',
 	rating: 3.9,   | <-- 삭제하겠습니다.
 },
  {
  // ... (생략)
  }
```

동일하게 페이지에는 시각적으로 에러는 없고, 콘솔을 보면 에러가 떴군요, 

![img](https://blog.kakaocdn.net/dn/mg3JD/btq8w5732Cz/XZnJkewoDX9nB7DBXZdSk0/img.jpg)

rating은 required이어야 한다고 나와있군요, 이처럼 isRequired를 추가하여 설정하게 되면 rating을 실수로 지우게 되거나 입력하지 않으면 콘솔창에 에러가 표시되어서 저희가 손쉽게 파악을 할 수 있게끔 해줍니다. 



또한 간단하지만 기억해줘야 할 것은, propTypes는 꼭 propType으로 이름을 지어야합니다, 예를 들어 sexyTypes라고 이름 지을 순 없습니다. 왜냐하면 이렇게 이름을 짓게 된다면 react가 이를 확인하지 않을 것이고, react가 아무것도 체크할 수 없기 때문입니다.



다음 단계로 넘어가기 전에 우선 위의 예제의 완성된 코드를 확인하겠습니다.

```javascript
import React from 'react';
import PropTypes from 'prop-types';

const foodILike = [
  {
    id: 1,
    name: 'Kimchi',
    image:
      'https://www.kgnews.co.kr/data/photos/20201249/art_16068076531596_c04e9e.jpg',
    rating: 5,
  },
  {
    id: 2,
    name: 'Pizza',
    image: 'https://cdn.dominos.co.kr/admin/upload/goods/20200311_5MGKbxlW.jpg',
    rating: 4.9,
  },
  {
    id: 3,
    name: 'Hamburger',
    image:
      'https://pds.joins.com/news/component/htmlphoto_mmdata/201412/08/htm_20141208135901788.jpg',
    rating: 4.8,
  },
  {
    id: 4,
    name: 'Chuggumi',
    image:
      'https://recipe1.ezmember.co.kr/cache/recipe/2019/03/05/52d2be99c015378a75c9db81362422c71.jpg',
    rating: 3.9,
  },
  {
    id: 5,
    name: 'Kebab',
    image: 'https://t1.daumcdn.net/cfile/blog/999AD23E5CD1475810',
    rating: 4.7,
  },
  {
    id: 6,
    name: 'Kimbab',
    image:
      'https://i.pinimg.com/originals/e6/6f/34/e66f340299ef6e9a1a90b3a5c0d6cabb.png',
    rating: 4.5,
  },
];

function Food({ name, picture, rating }) {
  return (
    <div>
      <h2>I like {name}</h2>
      <h4>{rating}/5.0</h4>
      <img src={picture} alt={name} />
    </div>
  );
}

Food.propTypes = {
  name: PropTypes.string.isRequired,
  picture: PropTypes.string.isRequired,
  rating: PropTypes.number.isRequired,
};

function App() {
  return (
    <div>
      {foodILike.map((dish) => (
        <Food
          key={dish.id}
          name={dish.name}
          picture={dish.image}
          rating={dish.rating}
        />
      ))}
    </div>
  );
}

export default App;
```



------



다음 예제 코드를 살펴보겠습니다.



```javascript
import React, { Component } from 'react';

class ParentComponent extends Component {
  render() {
    return (
      <div>
        <h1>I'm the parent component.</h1>
        <h3>
          <PersonComponent name="Winter" age={21} />
        </h3>
      </div>
    );
  }
}

export default ParentComponent;

const PersonComponent = (props) => {
  return (
    <div>
      <p>
        {props.name} - {props.age}
      </p>
    </div>
  );
};
```



예제 코드를 보면, PersonComponent가 있습니다. 또한 name과 age, 두개의 속성을 사용하여, 이에 따라 PersonComponent에 두개의 prop values를 전달해주고 있습니다.

페이지를 보면 다음과 같이 Winter - 21이 표시됩니다. 

![img](https://blog.kakaocdn.net/dn/boXlRZ/btq8z6r8ow1/mjWykKFRgUBZOkO5bnVSok/img.jpg)

여기에서, 만약 age에다가 number가 아닌 string인 Karina를 넣는다면 어떻게 될까요?  페이지를 보면 Karina가 정상적으로 나오는 것을 확인할 수 있지만, 오류는 따로 없고 잘 동작합니다. 

![img](https://blog.kakaocdn.net/dn/IhJ2M/btq8AQijW3Q/mOetNTmc5jvkqOgxB7lBrk/img.jpg)

하지만 age는 number type이여야 하죠? 따라서 우리는 우리가 원하지 않는 type이 오는 것을 막을 것입니다.

그렇게 하기 위해선, propTypes를 사용하면 됩니다. 다음과 같이 prop-types를 사용할 준비를 완료하기 위해 import prop-types를 해보겠습니다.또한 

PersonComponent.propTypes라는 object안에  다음과 같이 name과 age에 대한 prop values를 정의해야합니다.

```javascript
PersonComponent.propTypes = {
	name: PropTypes.string,
	age: PropTypes.number,
};
```

좋습니다. 우리의 로컬 호스트를 다시 살펴보면 이제 prop-types 값을 정의했습니다.

또한 console을 보면 error 메시지가 나와있습니다.



![img](https://blog.kakaocdn.net/dn/YIJku/btq8BDCzpZG/zsfpE3GrAGylgjFEkScbA1/img.jpg)

이제는 익숙한 메시지입니다. age가 숫자여야 하므로 오류가 있다고 말하고 있죠? 다시금 age 값을 숫자로 변경하겠습니다. 그리고 이제 보면 오류가 사라졌습니다. 

-------------



이제는 props을 객체로 전달할 때의 상황을 봐보겠습니다. 

위 예제 코드를 조금 수정하여 다음과 같은 예제 코드를 만들었습니다.

```javascript
import React, { Component } from 'react';
import PropTypes from prop-types';

const Person = {
	name: 'Winter',
	age: 21,
};

class ParentComponent extends Component {
  render() {
    return (
      <div>
        <h1>I'm the parent component.</h1>
        <h3>
          <PersonComponent person={Person} />
        </h3>
      </div>
    );
  }
}

export default ParentComponent;

const PersonComponent = (props) => {
  return (
    <div>
      <p>
        {props.person.name} - {props.person.age}
      </p>
    </div>
  );
};

PersonComponent.propTypes = {
	name: PropTypes.string,
	age: PropTypes.number,
};
```



페이지 상으로는 동일하게 사람의 이름과 나이의 값을 볼 수 있습니다.

![img](https://blog.kakaocdn.net/dn/bSCd7i/btq8BDJnbGQ/JLk4VPXcmmW2n2E4JOZic0/img.jpg)

하지만 이러한 상황에서, prop-types을 이전과 동일하게 사용하기 위해서는 Person object로 대체해줘야합니다.

```javascript
const PersonComponent = (props) => {
  return (
    <div>
      <p>
        {props.person.name} - {props.person.age}
      </p>
    </div>
  );
};

PersonComponent.propTypes = {
	person: PropTypes.object,
};
```

이렇게 말이죠, 따라서 다음과 같이 

```
class ParentComponent extends Component {
  render() {
    return (
      <div>
        <h1>I'm the parent component.</h1>
        <h3>
          <PersonComponent person={21} />
        </h3>
      </div>
    );
  }
}
```

prop에 숫자를 전달하면 오류가 발생합니다.

![img](https://blog.kakaocdn.net/dn/bsW1ua/btq8yc0NznU/XAoBDAUSZGPdsi8qQcrHm0/img.jpg)

괜찮아 보입니다. propTypes가 완벽하게 작동하는 것을 볼 수 있지만, 하지만 약간의 문제가 생깁니다. 

Person 객체 내부 안에 age를 Karina로 변경해봅시다.

```
const Person = {
	name: 'Winter',
	age: 'Karina',
};
```

 이렇게 age가 string 타입으로 대체되었음에도 불구하고, 아무 문제없이 작동하는 것이 보입니다. 그러나 이것은 우리가 원하는 것이 아니죠? 

![img](https://blog.kakaocdn.net/dn/bs9uOX/btq8ycNfenx/QRcKHqJ1MRTGaewppl55K1/img.jpg)

따라서 이런 문제때문에 propType에 객체 값을 사용하는 것은 좋지 않습니다. 하지만 다행히도 propTypes는 shape라는 다른 속성을 지원하며 이를 통해 이러한 문제를 해결할 수 있습니다. shape 속성은 객체에도 사용되고, 이를 통해 객체 내 값의 타입을 체크할 수 있습니다.

shape 속성은 다음과 같이 활용할 수 있습니다.

```javascript
PersonComponent.propTypes = {
    person: PropTypes.shape({
    	name: PropTypes.string,
		age: PropTypes.number,
    }),
};
```

이제 로컬 호스트로 돌아가 확인해보겠습니다. age에 대해서 오류를 잘 체크하고 있네요.

![img](https://blog.kakaocdn.net/dn/PBuaf/btq8Ct0HIvX/cktpdzXMK0dI08dICsjM4K/img.jpg)

그럼 다시금 age를 숫자로 설정해보겠습니다.



![img](https://blog.kakaocdn.net/dn/bSCd7i/btq8BDJnbGQ/JLk4VPXcmmW2n2E4JOZic0/img.jpg)



오류 없이 잘 동작하는 것을 확인할 수 있습니다.

따라서 shape 속성을 사용해서, shape 속성 내에서 객체 내부의 값을 확인하고 체크할 수 있습니다.

------





다음으로는 defaultProps에 대해서 알아보겠습니다.

가끔씩은 실수로 props 를 빠트려먹을때가 있습니다. 혹은, 특정 상황에 props 를 일부러 비워야 할 때도 있구요. 즉, defaultProps는 props 데이터 중에 값이 없는 경우 기본 값을 설정하는 방법입니다. 그러한 경우에, props 의 기본값을 설정해줄 수 있는데요, 그것이 바로 defaultProps 입니다

defaultProps 설정은 다음과 같이 합니다.

```javascript
import React, { Component } from 'react';

class MyName extends Component {
  static defaultProps = {
    name: '기본이름'
  }
  render() {
    return (
      <div>
        안녕하세요! 제 이름은 <b>{this.props.name}</b> 입니다.
      </div>
    );
  }
}

export default MyName;
```

이렇게 하면 만약에 <MyName /> 이런식으로 name 값을 생략해버리면 "기본이름" 이 나타나게 될 것입니다. 참고로, defaultProps 는 다음과 같은 형태로도 설정 할 수 있습니다.

```javascript
import React, { Component } from 'react';

class MyName extends Component {
  render() {
    return (
      <div>
        안녕하세요! 제 이름은 <b>{this.props.name}</b> 입니다.
      </div>
    );
  }
}

MyName.defaultProps = {
  name: '기본이름'
};

export default MyName;
```

우리가 곧 알아볼 함수형 컴포넌트에서 defaultProps 를 설정할땐 위 방식으로 하면 됩니다.



그렇다면 우리가 보던 예제코드에서 만약 name props에 isRequired를 설정하여 호출하게 하고, defaultProps도 설정하게 된다면 어떻게 될 지 궁금합니다. 

우선 다음과 같이 코드를 작성해보겠습니다.

```javascript
import React, { Component } from 'react';
import PropTypes from prop-types';

const Person = {
	name: 'Winter',
	age: 21,
};

class ParentComponent extends Component {
  render() {
    return (
      <div>
        <h1>I'm the parent component.</h1>
        <h3>
          <PersonComponent name="Winter" age={21} />
        </h3>
      </div>
    );
  }
}

export default ParentComponent;

const PersonComponent = (props) => {
  return (
    <div>
      <p>
        {props.name} - {props.age}
      </p>
    </div>
  );
};

PersonComponent.propTypes = {
	name: PropTypes.string,isRequired,
	age: PropTypes.number,
};

PersonComponent.defaultProps = {
    name: '기본이름',
};
```

 name prop에 isRequired를 설정하였고, defaultProps도 설정해논 상태입니다.

이러한 상태에서는 페이지가 정상적으로 동작합니다.

![img](https://blog.kakaocdn.net/dn/bSCd7i/btq8BDJnbGQ/JLk4VPXcmmW2n2E4JOZic0/img.jpg)

그렇다면 만약 name="Winter"을 삭제하면 어떻게 될까요?

다음과 같이 말이죠.

```
import React, { Component } from 'react';
import PropTypes from prop-types';

const Person = {
	name: 'Winter',
	age: 21,
};

class ParentComponent extends Component {
  render() {
    return (
      <div>
        <h1>I'm the parent component.</h1>
        <h3>
          <PersonComponent age={21} />
        </h3>
      </div>
    );
  }
}

export default ParentComponent;

const PersonComponent = (props) => {
  return (
    <div>
      <p>
        {props.name} - {props.age}
      </p>
    </div>
  );
};

PersonComponent.propTypes = {
	name: PropTypes.string,isRequired,
	age: PropTypes.number,
};

PersonComponent.defaultProps = {
    name: '기본이름',
};
```

이러한 상황에서도 로컬 호스트는 잘 동작합니다. person component에서 오류를 콘솔에 띄우는 대신에 '기본이름'이 페이지 상에서 표시됩니다. defaultProps에서 설정을 해놨기 때문이죠. 



![img](https://blog.kakaocdn.net/dn/W7J41/btq8AsaHPUf/xGUu6HtEyS2xKZ1xQb87Ok/img.jpg)

