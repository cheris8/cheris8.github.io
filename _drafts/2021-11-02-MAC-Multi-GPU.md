


Node: 컴퓨터 1대를 의미, system이라고도 함
Single Node Single GPU: 컴퓨터 1대에 GPU 1개
Single Node Multi GPU: 컴퓨터 1대에 GPU 여러개, 보통 4~8개 정도
Multi Node Multi GPU: 서버 컴퓨터 같이 컴퓨터가 여러대


torch.cuda.empty_cache()

사용하지 않은 GPU상 cache 정리
가용 메모리 확보
del과는 구분 필요
reset 대신 쓰기 좋은 함수

training loop에 tensor 확인

tensor로 처리된 변수는 GPU 상에 메모리를 사용
해당 변수 loop 안에, 연산에 있을 때 GPU에 computational graph를 생성(메모리 잠식)
.item, float을 사용해 python 기본객체로 만들어 쌓이는 것을 방지


del 명령어 사용

필요가 없어진 변수는 적절한 삭제가 필요
가능 batch size 실험해보기

학습시 OOM이 발생했다면 batch size를 1로 해서 실험해보기

torch.no_grad()

Inference 시점에서는 torch.no_grad()를 사용
backward pass로 인해 쌓이는 메모리에서 자유로움