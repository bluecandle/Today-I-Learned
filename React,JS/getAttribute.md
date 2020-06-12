# getAttribute from event
<li name = "abc">abc</li>
이렇게 있다고 가정하자. (li 태그 안에 name property 를 넣었다.) 이걸 이벤트 핸들러에서 event.currentTarget or event.target 에서 .name 으로 접근하려고 하면 안된다! 누군가한테는 당연한 이야기겠지만, 이제 알았다. 애초에 html tag 사용 시에 id, value 이런거 빼고 추가적으로 넣어본 적이 없는 것 같다.
 아무튼, 대신에 event.target.getAttribute('name') 이런식으로 해야한다!

