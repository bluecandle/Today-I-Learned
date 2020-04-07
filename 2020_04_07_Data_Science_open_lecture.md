# Title
Sight and Sound: Learning Speech from Self-Supervision

# Lecturer
Dr. Joon-Son Chung
Research Scientist, Naver R&D Center
이번 주 Data Science Seminar의 연사는 네이버의 정준선 박사입니다.
정준선 박사는 Oxford에서 박사를 하고, 네이버에서 컴퓨터 비전과 음성인식을 연구하고 있습니다.

# Supervised Leraning
각 데이터에 label 을 다는 작업이 필요한데, 그 과정의 비용의 문제.
그로 인해 대부분의 데이터셋이 특정 키워드 관련해서만 존재.
(http://www.image-net.org/)

# Cross-modal self-supervision
비디오는 오디오의 supervision 이 될 수 있고, 오디오는 비디오의 supervision 이 될 수 있다. 둘은 항상 correlate, synchronize 되어있다는 특징을 활용하는 것!
### concatenation
### embedding
거리를 사용하는 방법. 기타 소리와 다른 기타소리가 가깝다\멀다
=> 이런 것을 학습하면 => 다른 영역(악기) 에도 활용이 가능해지게 된다.

## self-supervision in speech
누가 말하는지 , 어떤 내용을 말하는지 분간이 가능하다?!
오디오와 비디오의 싱크가 맞는지도 분간이 가능하다?!