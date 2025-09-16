import streamlit as st
import base64

# --- 페이지 기본 설정 ---
st.set_page_config(
    page_title="다가오는 바다, 우리의 미래",
    page_icon="🌊",
    layout="wide"
)

# --- 세련된 배경과 커스텀 스타일 적용 ---
st.markdown("""
<style>
/* Streamlit 앱의 메인 배경 */
.stApp {
    background-image: linear-gradient(135deg, #001f4d, #000000);
    background-attachment: fixed;
    background-size: cover;
    color: #e0e0e0;
}

/* 헤더와 제목 색상 */
h1, h2, h3 {
    color: #ffffff;
}

/* 검색창 스타일 (여기서는 사용하지 않지만, 디자인 유지를 위해 남겨둠) */
.stTextInput > div > div > input {
    background-color: #000000;
    color: #FFFFFF;
    border: 1px solid #66ccff;
    border-radius: 20px;
    text-align: center;
}

/* 확장(expander) 컴포넌트 스타일 */
.stExpander {
    background-color: rgba(0, 31, 77, 0.7);
    border: 1px solid #66ccff;
    border-radius: 10px;
    text-align: center;
}
.stExpander header {
    color: #ffffff !important;
    font-weight: bold;
}

/* 구분선 색상 */
hr {
    background-color: #444444;
}

/* 정보 알림창 스타일 */
div[data-baseweb="alert"] {
    background-color: #000000 !important;
    color: #FFFFFF !important;
    border: 1px solid #66ccff !important;
    border-radius: 10px;
    text-align: center;
}

/* 모든 p 태그 색상 고정 */
p {
    color: #cccccc;
    font-size: 1.1rem;
    line-height: 1.6;
}

strong {
    color: #66ccff;
}

/* 이미지 스타일 */
.stImage img {
    border-radius: 10px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.5);
    margin-bottom: 1rem;
    border: 2px solid #66ccff;
}
</style>
""", unsafe_allow_html=True)


# --- 해수면 상승 관련 데이터 ---
# 각 주제를 뮤지컬 데이터 형식처럼 구성합니다.
sea_level_data = [
    {
        "title": "서론: 물러서는 땅, 다가오는 바다",
        "poster_url": "https://i.imgur.com/g0a1ceY.jpg", # 빙하 이미지
        "summary": "인류의 기술이 나날이 발전함과 동시에 세상은 황폐해져 가고 있습니다. 기온은 해마다 오르고, 북극과 남극의 빙하는 녹아내리며, 바다는 따뜻해지고 해수면은 조용히 그러나 확실하게 높아지고 있습니다. <strong>지금 이 순간에도 우리 삶의 터전은 서서히 잠식당하고 있는 것입니다.</strong> 이 대시보드는 해수면 상승의 심각성을 알리고, 우리가 선택해야 할 대처 방안을 제시하고자 합니다.",
        "details": {
            "주요 문제점": ["삶의 터전 상실", "식수 오염", "생태계 파괴", "경제적 손실"],
            "핵심 메시지": ["훗날 미래를 이끌어 갈 청소년 여러분이 이 문제를 외면한다면, 그 피해는 여러분의 세대가 고스란히 떠안게 될 것입니다."]
        }
    },
    {
        "title": "현실이 된 경고: 2050년의 대한민국",
        "poster_url": "https://i.imgur.com/uCgSH0S.png", # 한반도 침수 예상도
        "summary": "이 지도는 2050년을 가정해 해수면 상승으로 잠기게 될 대한민국의 주요 도시를 보여줍니다. 단순한 그림이 아니라, <strong>과학적 데이터와 시뮬레이션을 바탕으로 만들어진 미래의 경고장입니다.</strong> 인천국제공항, 부산의 주요 항만 시설, 그리고 서해안과 남해안의 넓은 농경지들이 물에 잠길 수 있다는 사실은 더 이상 영화 속 상상이 아닙니다.",
        "details": {
            "주요 침수 예상 지역": ["인천 (송도, 영종도)", "부산 (해운대, 강서구)", "전라북도 (새만금 일대)", "충청남도 (서산, 태안)"],
            "시사점": ["지금 우리가 아무런 행동을 하지 않는다면, 이 지도는 ‘예상도’가 아니라 ‘현실의 풍경’이 될 것입니다. 따라서 '내 문제'로 인식하고 작은 실천부터 시작해야 합니다."]
        }
    },
    {
        "title": "사례 연구: 지도에서 사라지는 나라, 투발루",
        "poster_url": "https://i.imgur.com/lM3XnKO.png", # 투발루 외교장관 연설
        "summary": "남태평양의 작은 섬나라 투발루는 평균 해발고도가 2~3m에 불과해 해수면 상승의 가장 직접적인 위협을 받고 있습니다. 바닷물이 섬 마을로 밀려들어와 농경지가 침수되고 식수원이 오염되는 일이 자주 발생하며, 주민들은 삶의 터전을 잃고 '환경 난민'이 될 위기에 처했습니다. <strong>투발루의 절박한 외침은 곧 전 세계 해안 도시들이 마주할 미래의 경고입니다.</strong>",
        "details": {
            "투발루의 현 상황": ["국토 침수 가속화", "식수 및 식량난 심화", "주민들의 강제 이주 문제 발생", "국가 소멸 위기"],
            "투발루 외교장관의 호소": ["2021년, 사이먼 코페 장관은 직접 바닷물에 들어가 연설하며 국제 사회에 기후 위기의 심각성을 알리고 즉각적인 행동을 촉구했습니다."]
        }
    }
]


# --- 앱 UI 구성 ---

# 제목과 부제
st.markdown("<h1 style='text-align: center;'>🌊 다가오는 바다, 우리의 미래</h1>", unsafe_allow_html=True)
st.markdown("<p style='text-align: center; color: #FFFFFF;'>해수면 상승의 위험성과 우리의 대처법을 알아보세요.</p>", unsafe_allow_html=True)


# --- 검색 기능 대신, 주요 콘텐츠를 바로 보여주도록 구성 ---
# 기존 뮤지컬 검색 로직은 제거하고, 모든 데이터를 순서대로 출력합니다.

st.divider()

if not sea_level_data:
    st.warning("표시할 콘텐츠가 없습니다.")
else:
    for item in sea_level_data:
        # 포스터(이미지) 표시 로직
        col1, col2 = st.columns([1, 2.5]) # 이미지와 정보 영역 비율 조정
        with col1:
            st.image(item["poster_url"], use_column_width=True)

        with col2:
            st.markdown(f"<h2 style='text-align: center; color: #66ccff;'>{item['title']}</h2>", unsafe_allow_html=True)
            st.markdown(f"<p style='text-align: justify;'>{item['summary']}</p>", unsafe_allow_html=True)

            with st.expander("자세히 보기"):
                for category, points in item['details'].items():
                    st.markdown(f"<p style='text-align: center;'><strong>{category}:</strong> {', '.join(points)}</p>", unsafe_allow_html=True)
        
        st.divider()

# --- 결론 및 대처 방안 섹션 추가 ---
st.markdown("<h2 style='text-align: center; color: #ffffff;'>🔑 결론: 우리에게 남은 선택과 행동</h2>", unsafe_allow_html=True)

col1, col2, col3 = st.columns(3)

with col1:
    with st.container(border=True):
        st.markdown("<h3 style='text-align: center;'>🌍 온실가스 감축</h3>", unsafe_allow_html=True)
        st.markdown("""
        - **에너지 전환:** 화석 연료 사용을 줄이고 태양광, 풍력 등 신재생에너지 사용 확대
        - **에너지 효율 개선:** 고효율 제품 사용 및 건물 단열 강화로 불필요한 에너지 소비 감축
        """, unsafe_allow_html=True)

with col2:
    with st.container(border=True):
        st.markdown("<h3 style='text-align: center;'>🏠 개인의 실천</h3>", unsafe_allow_html=True)
        st.markdown("""
        - **에너지 절약:** 사용하지 않는 전등 끄기, 대중교통 이용 등 생활 속 에너지 소비 줄이기
        - **자원 재활용:** 불필요한 소비를 줄이고, 쓰레기 분리배출을 철저히 하여 자원 낭비 방지
        """, unsafe_allow_html=True)

with col3:
    with st.container(border=True):
        st.markdown("<h3 style='text-align: center;'>📢 공동의 목소리</h3>", unsafe_allow_html=True)
        st.markdown("""
        - **관심과 참여:** 기후 변화 문제의 심각성을 인지하고, 관련 정책이나 캠페인에 관심 갖기
        - **적극적 행동:** 학교 환경 동아리, 지역 캠페인 등에 참여하여 더 많은 사람들의 동참 유도
        """, unsafe_allow_html=True)