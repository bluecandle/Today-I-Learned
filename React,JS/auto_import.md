# auto-import
React 에 한정되는 문제가 아니라, 어찌보면 VSC 등의 에디터를 사용하면서 발생할 수 있는 문제인것 같다. 작업 중, 의도하지 않은 moduel 의 import 가 자동으로 어찌어찌 이루어지는 경우, 갑자기 build 잘 되던 프로젝트가 빌드가 안되기 시작하는 경험을 했다.
패키지를 추가한 것도 아닌데, 멀쩡하던 프로젝트가 영문모를 이유로 build가 안되기 시작하면(예를 들어 unexpected token '<' 뭐 이런게 갑자기 보인다던지) 코드 파일에 의도치 않은 module 의 import가 이루어지지는 않았는지 확인하자.
