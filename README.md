### 프로젝트 수행 기간 (2024 / 02 / 02 - 2024 / 02 / 07)
# 프로젝트의 목적
###### 메가서울 : 메가서울의 모델인 메가시티는 핵심 도시를 중심으로 주변 지역과 교통·경제·문화를 연결한 인구 1000만 명 이상의 거대 도시 권역을 뜻한다.
<br>

- 서울 메가시티 통합 발표 후 부동산 투자에 관심을 갖는 모든 자본이 이목을 집중하고 있다.
- 정책으로 인해, 부동산 가격 상승의 직접적인 수혜를 받게 될 것이라고 예상되는 김포시 및 서울 주변 지역 거주민들은 서울 외곽의 거주민들보다 정책에 대한 기대감이 높다.
- 하지만 김포시와 서울시의 부동산 시장에, 정책이 끼치는 영향은 미미하다라고 보는 시각 또한 존재한다.  
- 본 프로젝트는 김포시와 서울시의 부동산 시장 데이터를 활용하여 부동산 시장 동향을 시각화하고, 
<br>부동산 시장에 대한 분석과 해석을 제공하여 사용자가 부동산 투자 결정을 하는 것에 도움을 줄 수 있는 대시보드를 개발하는 데 목적을 둔다. 
<br>이를 통해, 사용자는 김포시와 서울시의 부동산 시장 가격 변동과 거래량을 한 눈에 파악 할 수 있고, 부동산 시장에 대한 인사이트를 제공받는다. 
<br>이로 인해, 사용자는 김포시와 서울시의 부동산 시장을 쉽게 파악하고 부동산 투자 결정을 내리는 데 도움을 받을 것이다.

<br>

## 팀원 소개
#### 분석팀
  - 정혜원(팀장) : https://github.com/gookoc
  - 이형주 : https://github.com/honolulu12321
#### 개발팀
  - 김영기 : https://github.com/Y0un9Ki
  - 최진혁 : https://github.com/j2nhyeok
#### 기획팀
  - 안 별 : https://github.com/Byeol12341
  - 송인동 : https://github.com/Indongspace

<br>

## 본 프로젝트에서 사용한 주요 개발환경 요약 
  + Programming Languages : Python(ver. 3.11.7)
  + Web Framework : Streamlit, Jupyter lab

## 주요 라이브러리 버전
  + Pandas, Plotly.express , Numpy

## 데모페이지
- Streamlit에서 구현한 Demo는 다음과 같습니다.
  + https://teamminiproject-6qy4frxnwbz4ubp8dbhql7.streamlit.app/

 ## 주요 기능
 본 프로젝트에서 활용한 주요 메서드는 다음과 같습니다.
- **def seoul_sale(house_gbn_nm)**  1_부동산 가격 추이.py
1. 주택 유형(아파트, 오피스텔, 연립다세대)을 인자값으로 받아 해당하는 주택 유형의 서울시 월별 매매 가격을 그래프로 시각화하는 함수.

```
def seoul_sale(house_gbn_nm):
    df_seoul = pd.read_csv('서울전체/df_서울.csv')
    df_seoul = df_seoul.loc[df_seoul['주택유형'] == f'{house_gbn_nm}', :]
    df_seoul['계약년월'] = df_seoul['계약년월'].astype(str)

    st.subheader(f'서울 월별 {house_gbn_nm} 매매 평균 가격')

    fig = px.line(
        x=df_seoul['계약년월'].unique(),
        y=df_seoul.groupby('계약년월')['물건금액(만원)'].mean(),
        labels={'x': '월', 'y': f'평균 {house_gbn_nm} 가격(만원)'},
        template='plotly_dark',
        line_shape='linear',
        width=800,
        height=400,
        markers=True, )

    fig.update_yaxes(tickformat=",d")
    st.plotly_chart(fig)
```

- **def seoul_rent(rent_gbn, house_gbn_nm)** 1_부동산 가격 추이.py
1. 렌트 유형(전세, 월세)와 주택 유형(아파트, 오피스텔, 연립다세대)을 인자값으로 받아 해당하는 주택 유형의 서울시 월별 보증금 가격을 그래프로 시각화하는 함수.

```
def seoul_rent(rent_gbn, house_gbn_nm):
    df = pd.read_csv('서울전체/seoul_all.csv', encoding='euc_kr')
    df = df[(df['RENT_GBN'] == rent_gbn) & (df['HOUSE_GBN_NM'] == house_gbn_nm)]

    df = df[['RENT_GTN', 'CNTRCT_DE']]

    # 'RENT_GTN'을 월 별로 그룹화하여 평균값 계산
    monthly_data = df.groupby(df['CNTRCT_DE'].astype(str).str[:-2])['RENT_GTN'].mean().reset_index()

    # Streamlit 앱 시작
    st.subheader(f'서울 월별 {house_gbn_nm} {rent_gbn} 평균가격')

    # 선 그래프 그리기
    fig = px.line(
        monthly_data,
        x='CNTRCT_DE',
        y='RENT_GTN',
        labels={'CNTRCT_DE': '월', 'RENT_GTN': f'평균 {rent_gbn} 가격(만원)'},
        template='plotly_dark',  # Dark mode template
        line_shape='linear',  # Linear interpolation between points
        markers=True,  # Show markers on data points
        width=800,  # Width of the plot
        height=400,  # Height of the plot
    )

    # y-axis tick 포맷 변경 (5억 단위로)
    fig.update_yaxes(tickformat=",d")

    # Plotly 그래프를 Streamlit에 표시
    st.plotly_chart(fig)
```
- **def get_increase(df, temp)** 4_메가서울 발표 후 김포 평균 가격 변화율.py
1. df는 김포 매매 데이터프레임과 김포 전월세 데이터프레임으로 구분
2. temp는 매매/전세/월세로 구분
3. 해당된 df, temp를 기준으로 메가서울 발표 전후 김포시 평균 가격 변화율을 반환해줌.

 (예시) 김포 아파트의 매매 가격 변화율 <br>
df는 gimpo_apart, temp는 매매를 넣어줄 경우 메가서울 발표 전후 아파트 매매 증가율 -3%를 반환

```
def get_increase(df,temp):
    sentence = ""
    if temp == "매매":
        sentence ="거래금액(만원)"
    else:
        sentence = "보증금(만원)"

    df['계약년월'] = pd.to_datetime(df['계약년월'], format='%Y%m')

    # '보증금(만원)' 열의 쉼표 제거하고 숫자로 변환
    df[f'{sentence}'] = df[f'{sentence}'].replace({',': ''}, regex=True).astype(float)

    if temp == "매매":
        before_megaseoul = df[(df['계약년월'] <= '2023-09') & (df['계약년월'] >= '2023-06')]
        after_megaseoul = df[(df['계약년월'] <= '2024-01') & (df['계약년월'] >= '2023-10')]
    else:
    # 조건에 따라 데이터 필터링
        before_megaseoul = df[(df['전월세구분'] == f'{temp}') & (df['계약년월'] <= '2023-09') & (df['계약년월'] >= '2023-06')]
        after_megaseoul = df[(df['전월세구분'] == f'{temp}') & (df['계약년월'] <= '2024-01') & (df['계약년월'] >= '2023-10')]

    before_mean = before_megaseoul[f'{sentence}'].mean()
    after_mean = after_megaseoul[f'{sentence}'].mean()

    # 증가율 계산
    increase_rate = ((after_mean - before_mean) / before_mean) * 100

    return increase_rate
```

<br>

* 프로젝트의 폴더 및 파일 설명

| Folder/Files | Description |
|---|---|
|images|홈화면에 배치한 이미지 폴더|
|pages|Streamlit 사이드바 목록 페이지 폴더|
|김포데이터|김포시 부동산 데이터 폴더|
|서울전체|서울특별시 부동산 데이터 폴더|
|analysis(매매).ipynb|서울/김포 매매 분석 코드|
|analysis(전월세)|서울/김포 전월세 분석 코드|
|app.py|Streamlit 구현 코드|
|data_collect.ipynb|부동산 데이터 수집 코드|
|data_split.ipynb|수집한 데이터 중 필요한 부분 분할 및 병합 코드|
|requirements.txt|필요 라이브러리 기술 ('pip install -r requirements.txt' : 설치 명령어)|
|게시판.db|건의사항 저장 Database|

<br>

## 발표자료 
PPT
https://docs.google.com/presentation/d/18WeD68NM07TCBCJduix93Nn616BGuanRkyl0Tq3_CVM/edit?usp=drive_link

기획안
https://docs.google.com/document/d/1Apifd03AQ59bpAYK6zir9Hob5XiflluCkn1tDDfM2Gg/edit?usp=drive_link

대시보드
https://docs.google.com/presentation/d/17j37F8_-uDoz-gb4Tyewbb8Y7Ym_nYnIWsVp6ueEyNg/edit?usp=drive_link

<br>

## License
주소~ 서울 열린데이터광장 api

주소~ 국토교통부 실거래가 공개시스템 api
