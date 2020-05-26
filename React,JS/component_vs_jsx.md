# return 함수를 동반하는 component 와 jsx 객체 활용의 차이
모바일, 데스크탑 view 를 나누어서 작업할 필요가 있을 때,

return(
    <>
      <MobileView>
        {FinalDOM}
      </MobileView>
      <BrowserView>
        {FinalDOM}
      </BrowserView>
    </>
  )

 이런식으로 Fragment 안에 View Wrapper Component 를 넣어서 구분시켰다. 그런데, 위의 코드에 들어간 FinalDOM 이라는 JSX 코드가 들어간 object 가 아닌, return 함수를 동반하는 Component 를 사용했을 경우, 겉으로 보기에는 똑같다. 하지만, 시용자가 사용 동작을 했을 때, state 변경이 일어날 때마다 전체 페이지가 re-render 되는 상황이 벌어졌다. 즉, 페이지 하단에서 option 하나를 변경하는데 갑자기 화면이 최상단으로튀어버리는 것이다.
 처음에는 이게 도대체 왜 이런건지... 싶어서 여러가지 삽질을 하다가, 결국 FinalComponent 라는 이름으로모든 내용을 포함하는 component를 하나 더 거치는 것(return 함수 한 번 더 호출)이 원인일 것이라는 생각이 들었다. 그래서 return 함수 안에 있는 jsx 코드를 빼서 그냥 개별 jsx object 로 만들고 치환하였는데, 역시나... 원하는대로 동작하였다.
 component 를 구획별로 구분하는 것도 좋지만! 이렇게 전체를 하나로 묶어서 중간 단계를 하나 놓을 때는 조심해야겠다.
