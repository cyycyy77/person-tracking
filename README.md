# Person-Tracking with YOLOv4 & DeepSORT

> **Based on** [theAIGuysCode/yolov4-deepsort](https://github.com/theAIGuysCode/yolov4-deepsort)  
## 🚀 프로젝트 소개

- **목표**: 영상에서 사람만 탐지하고, 각 사람에 고유 ID를 부여해 추적.  
  사람이 사라지는 시점(초 단위)과 마지막으로 나타난 방위(3×3 격자, ‘북’, ‘남동’…)를 기록·표시.

## ✨ 주요 기능

1. **사람(person) 전용 객체 탐지** -> DeepSORT로 전
2. **ID Switching 완화**: 최소 5초(×FPS) 이상 등장한 ID만 최종 결과에 포함  
3. **동적 해상도 대응 격자 분할**: 입력 영상 크기에 관계없이 3×3 방위 분할  
4. **사라진 시점 및 방위**: 영상 종료 시 콘솔 또는 영상에 표시

## 📂 디렉터리 구조
```text
person-tracking/
├ object_tracker.py # 수정된 메인 스크립트
├ core/ # YOLOv4 유틸 (원본)
├ deep_sort/ # DeepSORT 구현 (원본)
├ tools/ # 지원 스크립트 (원본)
├ model_data/ 
├ checkpoints/
├ data/video/ # Raw input videos
├ outputs/ # Result videos
├ requirements.txt # Python 패키지
└ README.md
```


## 🛠️ 설치 및 실행 (원본 프로젝트 참조)

```bash
1. 레포지토리 클론

2. 가상환경 생성 (선택)

3. 필수 패키지 설치
pip install -r requirements.txt

4. 가중치 다운로드 후 변환
python save_model.py --model yolov4 

5. 실행 예시
python object_tracker.py \
  --video ./data/video/test.mp4 \
  --output ./outputs/result.avi \
  --weights ./checkpoints/yolov4-416 \
  --model yolov4
```

## 수정한 부분 (object_tracker.py)
1. get_region(): 해상도 자동 대응

2. 사람 전용 필터 (allowed_classes=['person'])

3. object_info 딕셔너리:

   - first_frame, last_frame, last_time, last_region, disappeared 저장

4. ID Switching 완화: 최소 지속 시간 필터

5. 최종 출력: 최종 등장 시각(시:분:초 형식), 최종 등장 방위

   - 사라진 시각을 시:분:초로 출력(영상 내 시간 기준)
   - 영상을 가상의 격자로 나누어 (8방위 + 중앙) 객체가 마지막으로 등장한 방위 출력
   - 영상 종료 후 콘솔창에 출력

## 📄 라이선스
원본 프로젝트(MIT License)를 준수합니다.
자세한 내용은 LICENSE 참조.
