# 필요한 라이브러리를 가져옵니다.
import io
import datetime
from urllib.request import urlopen

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import streamlit as st
import platform

# 운영체제에 맞는 한글 폰트 설정
# Matplotlib에서 한글이 깨지지 않도록 설정합니다.
if platform.system() == 'Windows':
    plt.rc('font', family='Malgun Gothic')
elif platform.system() == 'Darwin': # MacOS
    plt.rc('font', family='AppleGothic')
else: # Linux
    # Linux에서는 나눔고딕 폰트가 설치되어 있어야 합니다.
    plt.rc('font', family='NanumGothic')

# Matplotlib의 축 마이너스 부호 깨짐 방지
plt.rcParams['axes.unicode_minus'] = False

# --- 대시보드 페이지 설정 ---
st.set_page_config(
    page_title="다가오는 바다, 우리의 미래",
    page_icon="🌊",
    layout="wide"
)

# --- 설문조사 데이터 시각화 함수 ---
def create_survey_chart():
    """
    청소년 대상 해수면 상승 인식에 대한 가상 설문조사 데이터를 생성하고,
    이를 바탕으로 Seaborn 막대그래프를 그리는 함수입니다.
    """
    data = {
        '질문': [
            '해수면 상승의 원인을 알고 있나요?',
            '해수면 상승이 우리나라에 미치는 영향을 알고 있나요?',
            '해수면 상승 문제 해결을 위해 노력할 의향이 있나요?'
        ],
        '그렇다 (%)': [65, 55, 75],
        '아니다/잘 모른다 (%)': [35, 45, 25]
    }
    df = pd.DataFrame(data)
    df_melted = df.melt(id_vars='질문', var_name='응답', value_name='비율 (%)')

    fig, ax = plt.subplots(figsize=(10, 6))
    sns.barplot(data=df_melted, x='질문', y='비율 (%)', hue='응답', ax=ax, palette='coolwarm')

    ax.set_title('청소년 해수면 상승 인식 설문조사 (가상)', fontsize=16)
    ax.set_xlabel('')
    ax.set_ylabel('응답 비율 (%)', fontsize=12)
    ax.tick_params(axis='x', rotation=0)
    ax.legend(title='응답 종류')
    
    for p in ax.patches:
        ax.annotate(f'{p.get_height():.0f}%',
                    (p.get_x() + p.get_width() / 2., p.get_height()),
                    ha='center', va='center',
                    xytext=(0, 9),
                    textcoords='offset points')
    return fig

# --- 대시보드 본문 ---
st.title('물러서는 땅, 다가오는 바다: 해수면 상승의 위험과 우리만의 대처법')
st.caption(f"보고서 작성일: {datetime.date.today().strftime('%Y년 %m월 %d일')}")
st.markdown("---")

st.header('🌍 서론: 서서히 잠식당하는 우리의 터전')
st.markdown("""
인류의 기술이 나날이 발전함과 동시에 세상은 황폐해져 가고 있습니다.
기온은 해마다 오르고, 북극과 남극의 빙하는 녹아내리며, 남극에서는 아름다운 꽃을 볼 수 있게 되었습니다. 바다는 따뜻해지고 해수면은 조용히 그러나 확실하게 높아지고 있습니다.
**지금 이 순간에도 우리 삶의 터전은 서서히 잠식당하고 있는 것입니다.**

이 보고서는 아직 해수면 상승의 심각성을 와닿지 못하는 청소년들에게 그 위험을 알리고, 우리가 반드시 선택해야 할 대처 방안을 제시하고자 합니다. 훗날 가까운 미래에 세상을 이끌어 갈 청소년 여러분이 이 문제를 외면한다면, 결국 그 피해는 여러분의 세대가 고스란히 떠안게 될 것입니다.
""")
st.markdown("---")

st.header('📊 설문으로 보는 청소년의 인식')
st.write("해수면 상승에 대해 청소년들은 얼마나 심각하게 인지하고 있을까요? 아래는 '해수면 상승'에 대한 청소년들의 인식을 가상으로 나타낸 설문조사 결과입니다.")
survey_fig = create_survey_chart()
st.pyplot(survey_fig)
st.info("""
이처럼, 많은 청소년들이 문제 해결의 의지는 있지만, 아직 해수면 상승의 원인이나 우리나라에 미칠 영향에 대해서는 정확히 인지하지 못하는 경우가 많습니다. 지금부터 이 보고서를 통해 적절한 대처 방안이 이루어지지 않을 시의 피해 사례와 그 현실이 우리에게도 멀지 않았음을 알려주려 합니다.
""")
st.markdown("---")

st.header('🗺️ 본론 1: 2050년, 물에 잠길 우리의 미래')
col1, col2 = st.columns([1.5, 2])
with col1:
    st.image(
        "https://i.imgur.com/uCgSH0S.png",
        caption="2050년 해수면 상승 시 한반도 침수 예상도 (자료: Climate Central)"
    )
with col2:
    st.markdown("""
    위 지도는 2050년을 가정해 해수면 상승으로 잠기게 될 대한민국의 주요 도시와 세계 각국의 연안을 보여줍니다. 단순한 그림이 아니라, **과학적 데이터와 시뮬레이션을 바탕으로 만들어진 미래의 경고장**입니다.
    인천국제공항, 부산의 주요 항만 시설, 그리고 서해안과 남해안의 넓은 농경지들이 물에 잠길 수 있다는 사실은 더 이상 영화 속 상상이 아닙니다.
    지금 우리가 아무런 행동을 하지 않는다면, 2050년의 이 지도는 ‘예상도’가 아니라 ‘현실의 풍경’이 될 것입니다. 결국 그 피해를 고스란히 짊어지게 되는 세대가 바로 여러분입니다. 따라서 지금 이 순간부터 기후 변화와 해수면 상승 문제를 **‘내 문제’**로 인식하고, 일상 속 작은 실천부터 시작해야 합니다.
    """)
st.markdown("---")

st.header('🏝️ 본론 2: 지도에서 사라지는 나라, 투발루의 눈물')
st.markdown("본론 1에서 보았듯, 해수면 상승은 먼 미래의 가상이 아니라 실제로 여러 나라에서 심각한 피해를 일으키고 있습니다. 이러한 피해 사건들을 살펴보면 해수면 상승이 우리 생활과 안전에 어떤 영향을 주는지 더 잘 알 수 있습니다.")
st.subheader("1. 투발루 (Tuvalu): 21세기의 아틀란티스")
col3, col4 = st.columns(2)
with col3:
    st.markdown("""
    투발루는 남태평양에 있는 작은 섬나라로, 평균 해발고도가 2~3m밖에 되지 않아 해수면 상승에 가장 큰 위협을 받고 있습니다.
    이 때문에 주민들이 살 곳을 잃고, 호주나 뉴질랜드 등으로 이주하는 **‘환경 난민’** 문제가 생기고 있습니다. 전문가들은 앞으로 해수면이 계속 높아진다면 투발루라는 나라 자체가 지도에서 사라질 수 있다고 경고하고 있는 상황입니다.
    몇 년 전, 투발루의 외무장관은 직접 바닷물에 들어가 연설하며 국제 사회에 기후 위기의 심각성을 알렸습니다. 하지만 안타깝게도 여러 나라로부터 자국민 이민 요청을 거부당하는 등 어려운 상황에 처해 있습니다.
    """)
with col4:
    st.image(
        "https://i.imgur.com/lM3XnKO.png",
        caption="물에 잠긴 채로 기후변화 회의 연설을 하는 투발루 장관 (사진: Reuters)"
    )
st.markdown("---")

st.header('🔑 결론: 우리에게 남은 선택')
st.error("""
**해수면 상승은 더 이상 먼 미래의 이야기가 아닌, 우리 눈앞에 닥친 현실입니다.**
투발루의 절박한 외침은 곧 대한민국을 포함한 전 세계 해안 도시들이 마주할 미래의 경고입니다.
""")
st.subheader("그렇다면 우리는 무엇을 해야 할까요? 🤔")
tab1, tab2, tab3 = st.tabs(["🌍 국가/사회적 노력", "🏠 개인의 실천", "📢 우리의 제언"])
with tab1:
    st.header("온실가스 감축 및 해안 보호")
    st.markdown("""
    - **에너지 전환**: 화석 연료를 줄이고 신재생에너지 사용을 확대합니다.
    - **에너지 효율 개선**: 에너지 효율이 높은 제품을 사용하고, 건물 단열을 강화합니다.
    - **방파제 및 해안 방조제 건설**: 해안 지역에 물리적인 방어벽을 설치합니다.
    - **자연 해안선 복원**: 맹그로브 숲, 갯벌 등 자연 방어선을 복원합니다.
    """)
with tab2:
    st.header("일상 속 작은 실천")
    st.markdown("""
    - **에너지 절약**: 사용하지 않는 전등 끄기, 대중교통 이용하기.
    - **자원 재활용 및 소비 줄이기**: 불필요한 소비를 줄이고, 분리배출을 철저히 합니다.
    - **환경 문제에 대한 관심과 참여**: 관련 정책이나 캠페인에 관심을 갖고 참여합니다.
    """)
with tab3:
    st.header("공동의 목소리 내기")
    st.success("""
    해수면 상승은 개인의 노력을 넘어, **공동의 목소리를 내는 적극적인 행동**이 필요합니다.
    학교 환경 동아리나 지역 사회 캠페인에 참여하여 더 많은 사람들의 동참을 이끌어내야 합니다.
    """)