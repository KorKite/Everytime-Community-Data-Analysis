# 에브리타임 대학생 커뮤니티 강의평가 데이터 분석

### 강의평가
    에브리타임에는 강의평가 기능이 존재합니다. 대학에서 들은 강의에 대해서 별점(1점 ~ 5점)과 글로 작성하는 리뷰로 이뤄져 있습니다.
    리뷰를 작성할 때 별점을 무조건 부여해야 함으로 별점과 글로된 강의평을 연결지어 학습시키고자 하였습니다.

### 문제정의와 프로젝트 동기
    강의평가를 보게 되면 너무 내용이 복잡하여 무슨 내용을 중심으로 봐야하는지 어려울 때가 있습니다. 
    그래서 강의평을 중요한 내용을 중심으로 시각화하면 어떨까 생각하여 프로젝트를 시작하였습니다.
    더불어 강의평을 남길때, 별점을 어떻게 줄지 햇갈리거나 혹은 잘못된 별점으로 강의평 파악에 어려움이 생기게 되어 예측모델을 고안하게 되었습니다.
    기존의 강의평과 비슷한 수준에서 별점을 예측해주는 모델을 기획하고 TKinter를 통해 실제 사용하면서 쓸 수 있도록 적용해볼 계획입니다.
    해당 프로젝트는 성균관대학교 인공지능융합전공 데이터마이닝 수업 기말 프로젝트입니다.
    동일한 프로젝트에서 해당 내용의 소스코드 및 아이디어를 도용하는 것은 명백한 위법행위입니다.

### 작동구조
    (https://user-images.githubusercontent.com/50725139/81751524-93bb3880-94ea-11ea-9cea-bcdf3abba8c1.png)

### 데이터 수집
    에브리타임 강의평은 저작권에 걸려있기 때문에 무단으로 배포할 수 없습니다.
    연구목적으로 크롤링을 진행하였고, 사용한 데이터는 업로드하지 않습니다.
    돌아다니며 현재 5501개의 강의평과 리뷰를 크롤링하여 학습하였습니다.
    
### 데이터 전처리
    데이터 전처리의 경우 한국어 데이터를 기계가 인식할 수 있도록 변형시켜주는 과정입니다.
    쉽게 말해서 "좋다", "좋은" 과 같이 같은 뜻이지만 다르게 쓰이는 단어를 "좋다"로 어원을 찾아주는 과정입니다.
    한국어 데이터 전처리는 KoNLTK의 트위터(Okt())를 사용해주었습니다.
    처리한 데이터 중 의미가 담긴 pos 테그만 분리하여 추출해주었습니다. (조사(Josa), 어미(Eomi) 등을 제거해 준 것입니다)
    
### 데이터분석1 - WordCloud & Topic Modeling
    데이터 시각화를 위해 워드클라우드와 토픽모델링을 사용하였습니다.
    워드 클라우드의 경우 인공지능이 들어가지는 않았습니다. 
    단순히 한 강의평에 대해 빈도높게 등장한 단어를 기준으로 정렬하고 그것을 표현해준 것입니다.
    토픽모델링의 경우 좋지는 토픽들간의 중요한 단어들로 묶어서 단어로 토픽을 표현하는 것인데, 
    한국에선 효과가 좋지 않기에 시도만 해보았습니다.
    
### 강의평 예측하기
    기존의 강의평을 학습하여 새로운 강의평이 들어왔을 때, 그것의  예측하는 모델을 만들어보았습니다.
    5500개의 train data와 한 강의의 강의평가 135개를 test data로 사용하여 진행하였습니다.
    
### 예측 평가
    강의평에 대한 평가는 test데이터로 진행하였습니다.
    단순한 정확도 (맞은개수 / 전체개수)로 할 경우 문제가 발생합니다. 
    5점을 4점으로 예측한 것과 1점으로 예측한 것이 동일하게 평가되기 때문입니다.
    단순 정확도를 높이는 것도 중요하지만 저는 Root Mean Square Error를 높이는 것을 중요하게 생각했습니다.
    이는 평균 제곱근 오차로 (예측값 - 실제값)의 제곱의 루트(절대값)의 평균을 구하는 방식으로 
    예측값이 실제값과 얼마나 차이가 나는지에 집중하여 분석했습니다.
    다른 계산법을 적용해야하는 이유는 사람마다 별점을 주는 기준이 조금씩 다르고 
    테스트셋이 기존 데이터와 완전하게 같은 패턴으로 점수를 주지는 않았기 때문입니다.
    
### 예측모델 1 - Machine Learning Based Model
    처음엔 머신러닝 기반으로 모델링을 하였습니다.
    3개의 모델을 만들어주었습니다.
    1. CountVectorizer + MultinomialNB
    2. TfidfVectorizer + SVM
    3. TfidfVectorizer + XGBClassifier
    
    데이터양이 부족한 1800개 정도로 처음에 진행하였는데, 당시 정확도가 20% 정도로 매우 안좋았습니다.
    데이터를 5500개 정도 수집한 현재의 정확도는 45%정도로 올라갔습니다.
    RMSE의 경우 세 모델이 거의 차이가 없는 1.5정도로 나왔습니다. 
    즉 4점짜리 실제 강의평을 평균적으로 1.5정도의 오차를 가지고 예측한다는 것입니다.
    
### 예측모델2 - DeepLearning Based Model
    두번째는 딥러닝 기반의 모델입니다.
    망64개짜리 망 2개와 출력층 1개를 쌓아주었고 512개의 배치사이즈를 가지고 7번의 Epoch를 거처 학습하였습니다.
    동일한 조건에서 training시켜주었습니다.
    점수을 정확히 그 점수로 예측하는 단순 정확도는 42%정도였습니다.
    그러나, RMSE의 경우 0.88로 엄청난 성능 향상이 있었습니다.
    데이터를 더 수집하여 만든다면 폭발적인 예측력의 향상이 있을 것이라 예상됩니다.
    
### 예측모델 결론
    예측의 경우 딥러닝 기반의 모델의 성능이 월등히 뛰어나다는 것을 확인하였습니다.
    데이터가 5500개 정도밖에 없는데, 대략 100000개까지 수집하여 추가적으로 데이터를 분석해보도록 할 예정입니다.
