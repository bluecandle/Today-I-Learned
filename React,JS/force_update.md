# functional component 에서 강제 update
redux 에서 state 를 변경시키는 작업을 했는데 컴포넌트의 re-render 가 이루어지지 않는 경우, 클래스 컴포넌트에서는 forceUpdate() 함수를 사용하면 되겠지만, 함수형 컴포넌트에서는 그게 안된다. 그래서 해당 컴포넌트 안에 소속된 임의의 의미 없는 state 를 하나 생성하고, 그걸 변경해줘서 re-render 를 유도할 수 있다.

	const [rand, setRand] = useState(0)
	~~~
	setRand(Math.random())
	~~~

이런식으로.

