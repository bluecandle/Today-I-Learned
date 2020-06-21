# typos in actionTypes.js
후...

export const GET_ANALYSIS_DISPLAY_LIST_MAP = 'ANALYSIS/GET_ANALYSIS_DISPLAY_LIST_MAP';
export const SET_ANALYSIS_DISPLAY_LIST_MAP = 'ANALYSIS/SET_ANALYSIS_DISPLAY_LIST_MAP';

아무리 생각해도 무한 호출이 일어날 상황이 아닌데, useEffect hook 에서 무한 호출이 일어나고 있길래... 계속 찾아다니다가 actionType을 정의해놓은 파일에서 GET,SET 의 텍스트가 동일하게 되어있는 것을 발견하고 수정했다! 복붙할때 조심좀 하자
