<a href = 'https://www.44bits.io/ko/post/change-root-directory-by-using-chroot'>source</a>

# 컨테이너 기초 - chroot를 사용한 프로세스의 루트 디렉터리 격리
반면에 컨테이너 가상화는 하드웨어를 에뮬레이션하지 않고, 독립된 환경에서 실행된 것 처럼 보이는 특별한 제약이 가해진 프로세스를 실행합니다. 실제로 호스트 OS의 컨테이너는 단순히 호스트 OS의 프로세스입니다. 컨테이너는 도커 위에서 실행되는 것이 아니라, OS 위에서 실행되는 다른 프로세스들과 정확히 같은 계층에서 실행됩니다.

 컨테이너를 구현하기 위한(격리된 프로세스를 생성하기 위한) 여러가지 기능이 있습니다만, 그 중에서도 가장 오래되고 기본이 되는 것은 프로세스가 실행되는 루트를 변경하는 일입니다. chroot는 1979년 Version 7 Unix에서 시스템콜로 처음 구현된 기능으로 벌써 40년이나 된 기능입니다

 기본적으로는 chroot 명령어 뒤에 루트로 사용할 디렉터리를 지정하고, 그 뒤에 이 루트를 기반으로 실행할 애플리케이션의 경로를 지정하는 방식입니다. 참고로 chroot는 root 권한을 필요로 합니다.

    $ chroot /home/nacyot/new_root /bin/bash

# chroot의 동작 원리
chroot라 루트를 변경한다는 게 어떤 의미인지 알아보겠습니다. chroot는 Change Root Directory의 줄임말로 명령어 자체가 루트 디렉터리를 바꾼다는 뜻을 가지고 있습니다. 

왜냐면 프로세스 K에게는 /A가 최상위 디렉터리, 즉 /이기 때문에 그 위에 있는 경로를 표현할 방법 자체가 없습니다.
이처럼 루트 디렉터리를 변경하면 특정 프로세스(K)가 상위 디렉터리에 접근할 수 없도록 격리 시킬 수 있습니다. 정확히 이 역할을 하는 것이 chroot 명령어입니다.

# chroot 입문: 새로운 루트에서 프로그램 실행하기
    $ chroot /tmp/new_root /bin/bash
그런데  /bin/bash는 실행되지 않습니다. 시스템의 루트에 /bin/bash가 존재하더라도 이 명령은 실패합니다. 왜냐하면 chroot의 두번째 인자가 되는 프로그램은 기존 루트를 기반으로한 프로그램 경로가 아니기 때문입니다. /bin/bash는 새로운 루트 디렉터리 아래의 경로입니다. 따라서 이 프로그램은 이 명령어를 시스템의 /bin/bash가 아니라 /tmp/new_root/bin/bash에서 찾습니다. 심지어 /bin/bash는 보이지조차 않습니다. 아!

# bash: ldd로 의존성 탐색
어떤 프로그램은 프로그램 파일만으로 실행이 되기도 하지만, 많은 프로그램들은 시스템 상에 다른 파일에 의존하고 있습니다. 이를 동적 라이브러리라고 합니다. bash 또한 이러한 파일들에 대한 의존성을 가지고 있고, 파일 시스템 아래에 이 파일들이 존재하지 않으면 실행이 되지 않습니다. 어떤 프로그램이 의존하고 있는 라이브러리 파일을 추적할 수 있는 프로그램이 바로 ldd입니다. ldd는 다음과 같이 사용합니다.

### jailbreaking
이를 통해서 현재 실행중인 bash 프로세스의 작업 디렉터리가 /이고, 그 아래의 구조가 기존의 파일 시스템 아래의 /tmp/new_root와 같다는 것을 확인할 수 있습니다. 그렇다면 이 프로세스에서 /tmp/new_root 위로 접근하는 것이 가능할까요? 짧은 대답은 ’아니오’입니다.*

* 좀 더 긴 대답은 ’그럴 수도 있다’입니다. 이러한 시도를 탈옥jailbreaking이라고 합니다. 실제로 컨테이너의 보안 이슈에 있어서 아주 중요한 부분 중 하나입니다. 단, 여기서는 그러한 가능성에 대해서 따로 다루지는 않습니다.

