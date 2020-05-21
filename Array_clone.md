# 배열 복사
후... 
let temp1 = [1,2,3]
let temp2 = temp1;
let temp3 = [...temp1]

이렇게 하면, temp1 == temp2 이지만, temp1 != temp3 이다. 이게 이렇게 놓고보면 당연하게 느껴지겠지만, react-redux 에서 useSelector hook 을 사용할 때, 얻고자 하는 값이 배열인 경우, 무심코 값을 얻어서 다른 state 를 변경하는 것에 그대로 넣어버리면...두 state 가 동일시 되는 (메모리에서 같은 주소를 차지하고 있는) 상태가 된다!!! 결국, reducer 에서 action 에서 받은 값을 [...actions.list] 이런식으로 다루거나, 아예 입력할 때 조심을 하던지 해야한다.
