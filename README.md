import React, { useState, useEffect, useMemo } from 'react';
import { Calendar, MapPin, Clock, Users, ExternalLink, Filter, Heart, Search, Star, Award, Route, Bell, CheckCircle, AlertCircle, Calendar as CalendarIcon, DollarSign, Trophy, Target, TrendingUp, BookOpen } from 'lucide-react';

// 2026년 실제 대한민국 마라톤 대회 데이터
const marathon2026Data = [
  {
    id: 1,
    name: "2026 서울마라톤 (동아마라톤)",
    organizer: "동아일보, 서울특별시",
    date: "2026-03-15",
    location: "서울 광화문광장 → 잠실종합운동장",
    region: "서울",
    courses: ["풀코스(42.195km)", "10km"],
    registrationStart: "2025-12-01",
    registrationEnd: "2026-01-31",
    paymentStart: "2026-01-05",
    paymentEnd: "2026-01-31",
    fee: { 
      full: 75000, 
      "10k": 35000 
    },
    status: "접수예정",
    website: "https://seoul-marathon.com",
    features: ["IAAF 플래티넘 라벨", "국제공인코스", "완주메달", "기록증명서", "대회 티셔츠"],
    maxParticipants: 40000,
    difficulty: "고급",
    category: "메이저대회",
    recordRequired: true,
    recordRequirement: "풀코스 기록 필수 (2023.01.01 이후)",
    weatherExpected: "봄날씨, 평균기온 10-15°C",
    elevation: "평지 위주, 일부 언덕",
    trafficControl: "전면통제",
    parkingInfo: "대중교통 이용 권장",
    facilities: ["급수대 15개소", "화장실 20개소", "의료진 배치"],
    startTime: "08:00",
    timeLimit: { full: "6시간", "10k": "2시간" },
    awards: ["완주메달", "기록증", "완주증", "기념품"],
    specialNote: "국내 최고 권위 대회, 기록 제출 필수"
  },
  {
    id: 2,
    name: "2026 대구국제마라톤",
    organizer: "대구광역시, 대한육상연맹",
    date: "2026-02-22",
    location: "대구 두류공원 → 대구스타디움",
    region: "대구",
    courses: ["풀코스(42.195km)", "10km", "5km"],
    registrationStart: "2025-11-15",
    registrationEnd: "2025-12-31",
    paymentStart: "2025-12-01",
    paymentEnd: "2025-12-31",
    fee: { 
      full: 65000, 
      "10k": 30000,
      "5k": 20000
    },
    status: "접수중",
    website: "https://daegumarathon.daegu.go.kr",
    features: ["IAAF 골드 라벨", "국제대회", "페이스메이커", "라이브 중계"],
    maxParticipants: 35000,
    difficulty: "중급",
    category: "국제대회",
    recordRequired: false,
    weatherExpected: "겨울 날씨, 평균기온 0-8°C",
    elevation: "평탄한 도심코스",
    trafficControl: "전면통제",
    parkingInfo: "셔틀버스 운영",
    facilities: ["급수대 12개소", "화장실 15개소", "응급의료소"],
    startTime: "09:00",
    timeLimit: { full: "6시간", "10k": "1시간 30분", "5k": "1시간" },
    awards: ["완주메달", "대구 특산품", "기념 타올"],
    specialNote: "시즌 첫 메이저 대회, 기록 도전 최적"
  },
  {
    id: 3,
    name: "2026 부산국제마라톤",
    organizer: "부산광역시, KBS부산",
    date: "2026-04-05",
    location: "부산 해운대해수욕장 → 광안리해수욕장",
    region: "부산",
    courses: ["풀코스(42.195km)", "하프(21.0975km)", "10km"],
    registrationStart: "2026-01-01",
    registrationEnd: "2026-02-28",
    paymentStart: "2026-01-15",
    paymentEnd: "2026-02-28",
    fee: { 
      full: 70000, 
      half: 45000,
      "10k": 25000
    },
    status: "접수예정",
    website: "https://busanmarathon.com",
    features: ["해안 코스", "광안대교 통과", "관광 연계", "해산물 특식"],
    maxParticipants: 30000,
    difficulty: "중급",
    category: "관광대회",
    recordRequired: false,
    weatherExpected: "봄날씨, 평균기온 12-18°C",
    elevation: "해안 평지, 광안대교 오르막",
    trafficControl: "부분통제",
    parkingInfo: "해운대 공영주차장",
    facilities: ["급수대 18개소", "화장실 25개소", "관광안내소"],
    startTime: "07:30",
    timeLimit: { full: "6시간", half: "3시간", "10k": "1시간 30분" },
    awards: ["완주메달", "부산 어묵", "기념 가방"],
    specialNote: "해안 절경 코스, 관광 겸용 추천"
  },
  {
    id: 4,
    name: "2026 서울하프마라톤",
    organizer: "조선일보, 서울특별시",
    date: "2026-04-26",
    location: "서울 광화문광장 → 상암 월드컵공원",
    region: "서울",
    courses: ["하프(21.0975km)", "10km"],
    registrationStart: "2025-12-16",
    registrationEnd: "2026-01-08",
    paymentStart: "2026-01-05",
    paymentEnd: "2026-01-08",
    fee: { 
      half: 40000,
      "10k": 25000
    },
    status: "접수예정",
    website: "https://seoulhalfmarathon.com",
    features: ["한강 코스", "초보자 친화", "완주율 95%", "사진 서비스"],
    maxParticipants: 20000,
    difficulty: "초급",
    category: "입문대회",
    recordRequired: false,
    weatherExpected: "봄날씨, 평균기온 15-20°C",
    elevation: "평지 위주",
    trafficControl: "부분통제",
    parkingInfo: "지하철 이용 권장",
    facilities: ["급수대 10개소", "화장실 15개소", "포토존"],
    startTime: "07:30",
    timeLimit: { half: "3시간", "10k": "1시간 30분" },
    awards: ["완주메달", "완주증", "기념 타올"],
    specialNote: "초보자 입문용 최적 대회"
  },
  {
    id: 5,
    name: "2026 제주국제마라톤",
    organizer: "제주특별자치도, 제주일보",
    date: "2026-05-18",
    location: "제주 제주시청 → 한라산 어리목",
    region: "제주",
    courses: ["풀코스(42.195km)", "하프(21.0975km)", "10km", "5km"],
    registrationStart: "2026-02-01",
    registrationEnd: "2026-03-31",
    paymentStart: "2026-02-15",
    paymentEnd: "2026-03-31",
    fee: { 
      full: 80000, 
      half: 50000,
      "10k": 30000,
      "5k": 20000
    },
    status: "접수예정",
    website: "https://jejumarathon.com",
    features: ["해안도로", "한라산 전망", "관광 패키지", "제주 특산품"],
    maxParticipants: 15000,
    difficulty: "중급",
    category: "관광대회",
    recordRequired: false,
    weatherExpected: "온화한 날씨, 평균기온 18-23°C",
    elevation: "해안+산간 복합코스",
    trafficControl: "부분통제",
    parkingInfo: "셔틀버스 및 렌터카",
    facilities: ["급수대 20개소", "화장실 18개소", "관광버스"],
    startTime: "07:00",
    timeLimit: { full: "7시간", half: "3시간 30분", "10k": "2시간", "5k": "1시간" },
    awards: ["완주메달", "제주 감귤", "한라봉", "기념품"],
    specialNote: "관광+마라톤 최고 조합"
  },
  {
    id: 6,
    name: "2026 춘천마라톤",
    organizer: "조선일보, 춘천시",
    date: "2026-10-25",
    location: "강원 춘천 공지천교 → 의암호",
    region: "강원",
    courses: ["풀코스(42.195km)", "10km"],
    registrationStart: "2026-06-24",
    registrationEnd: "2026-07-09",
    paymentStart: "2026-07-01",
    paymentEnd: "2026-07-09",
    fee: { 
      full: 60000,
      "10k": 25000
    },
    status: "접수예정",
    website: "https://chuncheonmarathon.com",
    features: ["의암호 순환", "가을 단풍", "국제공인", "닭갈비 특식"],
    maxParticipants: 12000,
    difficulty: "초급",
    category: "풍경대회",
    recordRequired: false,
    weatherExpected: "가을 날씨, 평균기온 10-18°C",
    elevation: "호수 주변 평지",
    trafficControl: "부분통제",
    parkingInfo: "무료 주차장",
    facilities: ["급수대 15개소", "화장실 12개소", "단풍 포토존"],
    startTime: "09:00",
    timeLimit: { full: "6시간", "10k": "2시간" },
    awards: ["완주메달", "춘천 닭갈비", "막국수"],
    specialNote: "가을 단풍 절경, 평탄한 코스"
  },
  {
    id: 7,
    name: "2026 JTBC 서울마라톤",
    organizer: "JTBC, 중앙일보",
    date: "2026-11-15",
    location: "서울 여의도한강공원 → 반포한강공원",
    region: "서울",
    courses: ["하프(21.0975km)", "10km", "5km"],
    registrationStart: "2026-08-01",
    registrationEnd: "2026-09-30",
    paymentStart: "2026-08-15",
    paymentEnd: "2026-09-30",
    fee: { 
      half: 45000,
      "10k": 30000,
      "5k": 20000
    },
    status: "접수예정",
    website: "https://marathon.jtbc.com",
    features: ["방송 중계", "연예인 참가", "한강뷰", "라이브 음악"],
    maxParticipants: 25000,
    difficulty: "초급",
    category: "엔터테인먼트",
    recordRequired: false,
    weatherExpected: "가을 날씨, 평균기온 8-15°C",
    elevation: "한강변 평지",
    trafficControl: "부분통제",
    parkingInfo: "여의도 공영주차장",
    facilities: ["급수대 12개소", "화장실 20개소", "방송부스"],
    startTime: "08:00",
    timeLimit: { half: "3시간", "10k": "1시간 30분", "5k": "1시간" },
    awards: ["완주메달", "JTBC 굿즈", "방송 출연권"],
    specialNote: "연예인과 함께 달리는 특별한 경험"
  },
  {
    id: 8,
    name: "2026 인천송도국제마라톤",
    organizer: "인천광역시, 송도국제도시",
    date: "2026-09-20",
    location: "인천 송도 센트럴파크 → 송도컨벤시아",
    region: "인천",
    courses: ["풀코스(42.195km)", "하프(21.0975km)", "10km"],
    registrationStart: "2026-06-01",
    registrationEnd: "2026-07-31",
    paymentStart: "2026-06-15",
    paymentEnd: "2026-07-31",
    fee: { 
      full: 65000,
      half: 40000,
      "10k": 25000
    },
    status: "접수예정",
    website: "https://songdomarathon.com",
    features: ["미래도시", "바다 전망", "국제적 환경", "첨단시설"],
    maxParticipants: 18000,
    difficulty: "중급",
    category: "미래도시",
    recordRequired: false,
    weatherExpected: "가을 날씨, 평균기온 18-25°C",
    elevation: "신도시 평지",
    trafficControl: "전면통제",
    parkingInfo: "대형 주차장",
    facilities: ["급수대 16개소", "화장실 18개소", "컨벤션 시설"],
    startTime: "07:00",
    timeLimit: { full: "6시간", half: "3시간", "10k": "1시간 30분" },
    awards: ["완주메달", "송도 기념품", "호텔 할인권"],
    specialNote: "미래도시에서 달리는 특별한 경험"
  },
  {
    id: 9,
    name: "2026 울산마라톤",
    organizer: "울산광역시, 울산매일",
    date: "2026-10-11",
    location: "울산 태화강국가정원 → 울산대공원",
    region: "울산",
    courses: ["풀코스(42.195km)", "하프(21.0975km)", "10km", "5km"],
    registrationStart: "2026-07-01",
    registrationEnd: "2026-08-31",
    paymentStart: "2026-07-15",
    paymentEnd: "2026-08-31",
    fee: { 
      full: 55000,
      half: 35000,
      "10k": 20000,
      "5k": 15000
    },
    status: "접수예정",
    website: "https://ulsanmarathon.com",
    features: ["태화강변", "생태 친화", "연어 회귀", "자연 환경"],
    maxParticipants: 15000,
    difficulty: "초급",
    category: "생태대회",
    recordRequired: false,
    weatherExpected: "가을 날씨, 평균기온 12-20°C",
    elevation: "강변 평지",
    trafficControl: "부분통제",
    parkingInfo: "태화강 주차장",
    facilities: ["급수대 14개소", "화장실 16개소", "생태 전시관"],
    startTime: "08:30",
    timeLimit: { full: "6시간", half: "3시간", "10k": "1시간 30분", "5k": "1시간" },
    awards: ["완주메달", "울산 특산품", "생태 기념품"],
    specialNote: "자연과 함께하는 힐링 마라톤"
  },
  {
    id: 10,
    name: "2026 전주마라톤",
    organizer: "전라북도, 전주시",
    date: "2026-04-19",
    location: "전주 한옥마을 → 전주천 → 덕진공원",
    region: "전북",
    courses: ["하프(21.0975km)", "10km", "5km"],
    registrationStart: "2026-01-20",
    registrationEnd: "2026-03-15",
    paymentStart: "2026-02-01",
    paymentEnd: "2026-03-15",
    fee: { 
      half: 38000,
      "10k": 23000,
      "5k": 18000
    },
    status: "접수예정",
    website: "https://jeonjumarathon.com",
    features: ["한옥마을", "전통문화", "비빔밥", "전주천"],
    maxParticipants: 10000,
    difficulty: "초급",
    category: "문화대회",
    recordRequired: false,
    weatherExpected: "봄날씨, 평균기온 13-19°C",
    elevation: "도심+천변 평지",
    trafficControl: "부분통제",
    parkingInfo: "한옥마을 주차장",
    facilities: ["급수대 10개소", "화장실 12개소", "문화 체험관"],
    startTime: "08:00",
    timeLimit: { half: "3시간", "10k": "1시간 30분", "5k": "1시간" },
    awards: ["완주메달", "전주 비빔밥", "한지 공예품"],
    specialNote: "전통문화와 함께하는 마라톤"
  }
];

// 참가신청 관리 데이터 구조
const initialApplications = [];

const KoreaMarathon2026App = () => {
  // 상태 관리
  const [marathons, setMarathons] = useState(marathon2026Data);
  const [filteredMarathons, setFilteredMarathons] = useState(marathon2026Data);
  const [searchTerm, setSearchTerm] = useState('');
  const [filterStatus, setFilterStatus] = useState('전체');
  const [filterRegion, setFilterRegion] = useState('전체');
  const [filterDistance, setFilterDistance] = useState('전체');
  const [filterDifficulty, setFilterDifficulty] = useState('전체');
  const [selectedMarathon, setSelectedMarathon] = useState(null);
  const [favorites, setFavorites] = useState(() => {
    const saved = localStorage.getItem('marathon-favorites-2026');
    return saved ? JSON.parse(saved) : [];
  });
  const [applications, setApplications] = useState(() => {
    const saved = localStorage.getItem('marathon-applications-2026');
    return saved ? JSON.parse(saved) : initialApplications;
  });
  const [showFilters, setShowFilters] = useState(false);
  const [viewMode, setViewMode] = useState('list'); // list, calendar, applications, statistics
  const [notifications, setNotifications] = useState([]);

  // 필터링 로직
  useEffect(() => {
    let filtered = marathons;

    if (searchTerm) {
      filtered = filtered.filter(marathon =>
        marathon.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
        marathon.location.toLowerCase().includes(searchTerm.toLowerCase()) ||
        marathon.region.toLowerCase().includes(searchTerm.toLowerCase())
      );
    }

    if (filterStatus !== '전체') {
      filtered = filtered.filter(marathon => marathon.status === filterStatus);
    }

    if (filterRegion !== '전체') {
      filtered = filtered.filter(marathon => marathon.region === filterRegion);
    }

    if (filterDistance !== '전체') {
      filtered = filtered.filter(marathon =>
        marathon.courses.some(course => course.includes(filterDistance))
      );
    }

    if (filterDifficulty !== '전체') {
      filtered = filtered.filter(marathon => marathon.difficulty === filterDifficulty);
    }

    setFilteredMarathons(filtered);
  }, [searchTerm, filterStatus, filterRegion, filterDistance, filterDifficulty, marathons]);

  // 로컬스토리지 저장
  useEffect(() => {
    localStorage.setItem('marathon-favorites-2026', JSON.stringify(favorites));
  }, [favorites]);

  useEffect(() => {
    localStorage.setItem('marathon-applications-2026', JSON.stringify(applications));
  }, [applications]);

  // 즐겨찾기 토글
  const toggleFavorite = (marathonId) => {
    setFavorites(prev =>
      prev.includes(marathonId)
        ? prev.filter(id => id !== marathonId)
        : [...prev, marathonId]
    );
  };

  // 참가신청 관리
  const addApplication = (marathonId, courseType, applicationData) => {
    const newApplication = {
      id: Date.now(),
      marathonId,
      courseType,
      applicationDate: new Date().toISOString(),
      status: '신청완료',
      ...applicationData
    };
    
    setApplications(prev => [...prev, newApplication]);
    
    // 알림 추가
    const marathon = marathons.find(m => m.id === marathonId);
    setNotifications(prev => [...prev, {
      id: Date.now(),
      type: 'success',
      title: '참가신청 완료',
      message: `${marathon.name} ${courseType} 참가신청이 완료되었습니다.`,
      timestamp: new Date().toISOString()
    }]);
  };

  const updateApplicationStatus = (applicationId, status) => {
    setApplications(prev =>
      prev.map(app =>
        app.id === applicationId ? { ...app, status } : app
      )
    );
  };

  const removeApplication = (applicationId) => {
    setApplications(prev => prev.filter(app => app.id !== applicationId));
  };

  // 통계 계산
  const statistics = useMemo(() => {
    const now = new Date();
    const thisMonth = now.getMonth();
    const thisYear = now.getFullYear();

    return {
      totalRaces: marathons.length,
      activeRaces: marathons.filter(m => m.status === '접수중').length,
      upcomingRaces: marathons.filter(m => m.status === '접수예정').length,
      myApplications: applications.length,
      myFavorites: favorites.length,
      totalCapacity: marathons.reduce((sum, m) => sum + m.maxParticipants, 0),
      avgFee: Math.round(
        marathons.reduce((sum, m) => {
          const fees = Object.values(m.fee);
          return sum + Math.min(...fees);
        }, 0) / marathons.length
      ),
      regionStats: marathons.reduce((acc, m) => {
        acc[m.region] = (acc[m.region] || 0) + 1;
        return acc;
      }, {}),
      monthlyStats: marathons.reduce((acc, m) => {
        const month = new Date(m.date).getMonth();
        const monthName = ['1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월'][month];
        acc[monthName] = (acc[monthName] || 0) + 1;
        return acc;
      }, {})
    };
  }, [marathons, applications, favorites]);

  // 참가신청 가이드 생성
  const getRegistrationGuide = (marathon) => {
    const steps = [
      {
        step: 1,
        title: "대회 정보 확인",
        description: `${marathon.name} 상세 정보와 참가 자격을 확인하세요.`,
        detail: marathon.recordRequired ? "기록 증명서가 필요한 대회입니다." : "누구나 참가 가능합니다."
      },
      {
        step: 2,
        title: "접수 기간 확인",
        description: `접수: ${formatDate(marathon.registrationStart)} ~ ${formatDate(marathon.registrationEnd)}`,
        detail: `결제: ${formatDate(marathon.paymentStart)} ~ ${formatDate(marathon.paymentEnd)}`
      },
      {
        step: 3,
        title: "공식 홈페이지 접속",
        description: `${marathon.website}에서 참가신청을 진행하세요.`,
        detail: "선착순 마감되는 경우가 많으니 서둘러 신청하세요."
      },
      {
        step: 4,
        title: "종목 및 개인정보 입력",
        description: `참가 종목: ${marathon.courses.join(', ')}`,
        detail: "이름, 생년월일, 연락처, 비상연락처, 의료 정보 등을 정확히 입력하세요."
      },
      {
        step: 5,
        title: "참가비 결제",
        description: `참가비: ${Object.entries(marathon.fee).map(([k, v]) => `${k} ${v.toLocaleString()}원`).join(', ')}`,
        detail: "신용카드, 계좌이체, 무통장 입금 등 다양한 결제 방법 지원"
      },
      {
        step: 6,
        title: "접수 확인 및 준비",
        description: "접수 완료 후 이메일/SMS로 확인 메시지를 받고 대회를 준비하세요.",
        detail: "대회 1주일 전 안내 메시지와 함께 번호표 수령 안내를 받게 됩니다."
      }
    ];

    return steps;
  };

  // 유틸리티 함수들
  const formatDate = (dateString) => {
    return new Date(dateString).toLocaleDateString('ko-KR', {
      year: 'numeric',
      month: 'long',
      day: 'numeric',
      weekday: 'short'
    });
  };

  const getDaysUntil = (dateString) => {
    const target = new Date(dateString);
    const now = new Date();
    const diffTime = target - now;
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
    return diffDays;
  };

  const getStatusColor = (status) => {
    switch (status) {
      case '접수중': return 'bg-green-100 text-green-800 border-green-200';
      case '접수예정': return 'bg-blue-100 text-blue-800 border-blue-200';
      case '접수마감': return 'bg-gray-100 text-gray-800 border-gray-200';
      default: return 'bg-gray-100 text-gray-800 border-gray-200';
    }
  };

  const getDifficultyColor = (difficulty) => {
    switch (difficulty) {
      case '초급': return 'bg-green-100 text-green-800';
      case '중급': return 'bg-yellow-100 text-yellow-800';
      case '고급': return 'bg-red-100 text-red-800';
      default: return 'bg-gray-100 text-gray-800';
    }
  };

  const getCategoryIcon = (category) => {
    switch (category) {
      case '메이저대회': return <Trophy className="w-4 h-4" />;
      case '국제대회': return <Star className="w-4 h-4" />;
      case '관광대회': return <MapPin className="w-4 h-4" />;
      case '입문대회': return <Target className="w-4 h-4" />;
      default: return <Award className="w-4 h-4" />;
    }
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-50 via-white to-green-50">
      {/* 헤더 */}
      <header className="bg-white shadow-lg border-b-4 border-blue-500 sticky top-0 z-50">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
          <div className="flex items-center justify-between">
            <div className="flex items-center space-x-3">
              <div className="bg-gradient-to-r from-blue-600 to-green-600 p-3 rounded-full">
                <Route className="w-8 h-8 text-white" />
              </div>
              <div>
                <h1 className="text-2xl font-bold text-gray-900">2026 대한민국 마라톤</h1>
                <p className="text-gray-600 text-sm">완벽한 참가신청 관리 시스템</p>
              </div>
            </div>
            
            {/* 네비게이션 */}
            <nav className="flex space-x-1">
              {[
                { key: 'list', label: '대회목록', icon: <BookOpen className="w-4 h-4" /> },
                { key: 'calendar', label: '캘린더', icon: <CalendarIcon className="w-4 h-4" /> },
                { key: 'applications', label: '참가신청', icon: <CheckCircle className="w-4 h-4" /> },
                { key: 'statistics', label: '통계', icon: <TrendingUp className="w-4 h-4" /> }
              ].map(nav => (
                <button
                  key={nav.key}
                  onClick={() => setViewMode(nav.key)}
                  className={`flex items-center px-3 py-2 rounded-lg text-sm font-medium transition-colors ${
                    viewMode === nav.key
                      ? 'bg-blue-500 text-white'
                      : 'text-gray-600 hover:text-gray-900 hover:bg-gray-100'
                  }`}
                >
                  {nav.icon}
                  <span className="ml-1 hidden sm:block">{nav.label}</span>
                </button>
              ))}
            </nav>
          </div>
        </div>
      </header>

      {/* 알림 */}
      {notifications.length > 0 && (
        <div className="fixed top-20 right-4 z-40 space-y-2">
          {notifications.slice(-3).map(notification => (
            <div
              key={notification.id}
              className="bg-white border-l-4 border-green-500 p-4 shadow-lg rounded-lg max-w-sm"
            >
              <div className="flex items-start">
                <CheckCircle className="w-5 h-5 text-green-500 mt-0.5" />
                <div className="ml-3">
                  <h4 className="text-sm font-medium text-gray-900">{notification.title}</h4>
                  <p className="text-sm text-gray-600">{notification.message}</p>
                </div>
                <button
                  onClick={() => setNotifications(prev => prev.filter(n => n.id !== notification.id))}
                  className="ml-auto text-gray-400 hover:text-gray-600"
                >
                  ×
                </button>
              </div>
            </div>
          ))}
        </div>
      )}

      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
        
        {/* 통계 대시보드 */}
        <div className="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4 mb-8">
          <div className="bg-white rounded-xl p-4 shadow-lg border border-gray-200">
            <div className="text-2xl font-bold text-blue-600">{statistics.totalRaces}</div>
            <div className="text-sm text-gray-600">전체 대회</div>
          </div>
          <div className="bg-white rounded-xl p-4 shadow-lg border border-gray-200">
            <div className="text-2xl font-bold text-green-600">{statistics.activeRaces}</div>
            <div className="text-sm text-gray-600">접수중</div>
          </div>
          <div className="bg-white rounded-xl p-4 shadow-lg border border-gray-200">
            <div className="text-2xl font-bold text-orange-600">{statistics.upcomingRaces}</div>
            <div className="text-sm text-gray-600">접수예정</div>
          </div>
          <div className="bg-white rounded-xl p-4 shadow-lg border border-gray-200">
            <div className="text-2xl font-bold text-purple-600">{statistics.myApplications}</div>
            <div className="text-sm text-gray-600">내 참가신청</div>
          </div>
          <div className="bg-white rounded-xl p-4 shadow-lg border border-gray-200">
            <div className="text-2xl font-bold text-red-600">{statistics.myFavorites}</div>
            <div className="text-sm text-gray-600">즐겨찾기</div>
          </div>
          <div className="bg-white rounded-xl p-4 shadow-lg border border-gray-200">
            <div className="text-2xl font-bold text-gray-600">{statistics.avgFee.toLocaleString()}원</div>
            <div className="text-sm text-gray-600">평균 참가비</div>
          </div>
        </div>

        {/* 뷰 모드에 따른 컨텐츠 렌더링 */}
        {viewMode === 'list' && (
          <>
            {/* 검색 및 필터 */}
            <div className="mb-8">
              <div className="flex flex-col sm:flex-row gap-4 mb-4">
                <div className="relative flex-1">
                  <Search className="absolute left-3 top-3 w-5 h-5 text-gray-400" />
                  <input
                    type="text"
                    placeholder="대회명, 지역으로 검색..."
                    className="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
                    value={searchTerm}
                    onChange={(e) => setSearchTerm(e.target.value)}
                  />
                </div>
                <button
                  onClick={() => setShowFilters(!showFilters)}
                  className="flex items-center px-6 py-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 transition-colors"
                >
                  <Filter className="w-5 h-5 mr-2" />
                  필터
                </button>
              </div>

              {showFilters && (
                <div className="bg-white p-6 rounded-xl shadow-lg border border-gray-200 mb-6">
                  <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-2">접수 상태</label>
                      <select
                        value={filterStatus}
                        onChange={(e) => setFilterStatus(e.target.value)}
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
                      >
                        <option value="전체">전체</option>
                        <option value="접수중">접수중</option>
                        <option value="접수예정">접수예정</option>
                        <option value="접수마감">접수마감</option>
                      </select>
                    </div>
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-2">지역</label>
                      <select
                        value={filterRegion}
                        onChange={(e) => setFilterRegion(e.target.value)}
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
                      >
                        <option value="전체">전체</option>
                        <option value="서울">서울</option>
                        <option value="부산">부산</option>
                        <option value="대구">대구</option>
                        <option value="인천">인천</option>
                        <option value="제주">제주</option>
                        <option value="강원">강원</option>
                        <option value="전북">전북</option>
                        <option value="울산">울산</option>
                      </select>
                    </div>
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-2">거리</label>
                      <select
                        value={filterDistance}
                        onChange={(e) => setFilterDistance(e.target.value)}
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
                      >
                        <option value="전체">전체</option>
                        <option value="풀코스">풀코스</option>
                        <option value="하프">하프</option>
                        <option value="10km">10km</option>
                        <option value="5km">5km</option>
                      </select>
                    </div>
                    <div>
                      <label className="block text-sm font-medium text-gray-700 mb-2">난이도</label>
                      <select
                        value={filterDifficulty}
                        onChange={(e) => setFilterDifficulty(e.target.value)}
                        className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
                      >
                        <option value="전체">전체</option>
                        <option value="초급">초급</option>
                        <option value="중급">중급</option>
                        <option value="고급">고급</option>
                      </select>
                    </div>
                  </div>
                </div>
              )}
            </div>

            {/* 대회 목록 */}
            {selectedMarathon ? (
              /* 상세보기 */
              <div className="bg-white rounded-xl shadow-lg border border-gray-200 overflow-hidden">
                <div className="bg-gradient-to-r from-blue-500 to-green-500 p-6 text-white">
                  <button
                    onClick={() => setSelectedMarathon(null)}
                    className="mb-4 text-white hover:text-blue-200 flex items-center"
                  >
                    ← 뒤로가기
                  </button>
                  <div className="flex justify-between items-start">
                    <div>
                      <h2 className="text-3xl font-bold mb-2">{selectedMarathon.name}</h2>
                      <p className="text-blue-100 text-lg">{selectedMarathon.organizer}</p>
                      <div className="flex items-center mt-2">
                        {getCategoryIcon(selectedMarathon.category)}
                        <span className="ml-2">{selectedMarathon.category}</span>
                      </div>
                    </div>
                    <div className="text-right">
                      <div className="text-2xl font-bold">{getDaysUntil(selectedMarathon.date)}일 전</div>
                      <div className="text-sm text-blue-200">대회까지</div>
                    </div>
                  </div>
                </div>

                <div className="p-6">
                  {/* 기본 정보 */}
                  <div className="grid md:grid-cols-2 gap-6 mb-8">
                    <div className="space-y-4">
                      <div className="flex items-center text-gray-600">
                        <Calendar className="w-5 h-5 mr-3 text-blue-500" />
                        <span className="font-medium">대회일자:</span>
                        <span className="ml-2">{formatDate(selectedMarathon.date)} {selectedMarathon.startTime}</span>
                      </div>
                      
                      <div className="flex items-center text-gray-600">
                        <MapPin className="w-5 h-5 mr-3 text-blue-500" />
                        <span className="font-medium">코스:</span>
                        <span className="ml-2">{selectedMarathon.location}</span>
                      </div>
                      
                      <div className="flex items-center text-gray-600">
                        <Users className="w-5 h-5 mr-3 text-blue-500" />
                        <span className="font-medium">참가정원:</span>
                        <span className="ml-2">{selectedMarathon.maxParticipants.toLocaleString()}명</span>
                      </div>

                      <div className="flex items-center text-gray-600">
                        <Clock className="w-5 h-5 mr-3 text-blue-500" />
                        <span className="font-medium">제한시간:</span>
                        <span className="ml-2">{Object.entries(selectedMarathon.timeLimit).map(([k, v]) => `${k}: ${v}`).join(', ')}</span>
                      </div>
                    </div>

                    <div className="space-y-4">
                      <div>
                        <span className="font-medium text-gray-600">참가종목:</span>
                        <div className="mt-2 flex flex-wrap gap-2">
                          {selectedMarathon.courses.map((course, idx) => (
                            <span key={idx} className="px-3 py-1 bg-blue-100 text-blue-800 rounded-full text-sm font-medium">
                              {course}
                            </span>
                          ))}
                        </div>
                      </div>

                      <div>
                        <span className="font-medium text-gray-600">참가비:</span>
                        <div className="mt-2 space-y-1">
                          {Object.entries(selectedMarathon.fee).map(([type, price]) => (
                            <div key={type} className="text-sm flex justify-between">
                              <span className="text-gray-600">{type}:</span>
                              <span className="font-semibold text-green-600">{price.toLocaleString()}원</span>
                            </div>
                          ))}
                        </div>
                      </div>

                      <div>
                        <span className="font-medium text-gray-600">난이도:</span>
                        <span className={`ml-2 px-2 py-1 rounded-full text-xs font-medium ${getDifficultyColor(selectedMarathon.difficulty)}`}>
                          {selectedMarathon.difficulty}
                        </span>
                      </div>
                    </div>
                  </div>

                  {/* 접수 정보 */}
                  <div className="mb-8 p-4 bg-yellow-50 border border-yellow-200 rounded-lg">
                    <h3 className="text-lg font-semibold text-yellow-800 mb-3">📅 접수 일정</h3>
                    <div className="grid md:grid-cols-2 gap-4 text-sm">
                      <div>
                        <strong className="text-yellow-700">참가 신청:</strong><br/>
                        {formatDate(selectedMarathon.registrationStart)} ~ {formatDate(selectedMarathon.registrationEnd)}
                      </div>
                      <div>
                        <strong className="text-yellow-700">참가비 결제:</strong><br/>
                        {formatDate(selectedMarathon.paymentStart)} ~ {formatDate(selectedMarathon.paymentEnd)}
                      </div>
                    </div>
                    {selectedMarathon.recordRequired && (
                      <div className="mt-3 text-sm text-red-600">
                        <AlertCircle className="w-4 h-4 inline mr-1" />
                        {selectedMarathon.recordRequirement}
                      </div>
                    )}
                  </div>

                  {/* 대회 특징 */}
                  <div className="mb-8">
                    <h3 className="text-lg font-semibold text-gray-900 mb-3">🏆 대회 특징</h3>
                    <div className="flex flex-wrap gap-2">
                      {selectedMarathon.features.map((feature, idx) => (
                        <span key={idx} className="px-3 py-2 bg-green-50 text-green-700 rounded-lg text-sm border border-green-200">
                          <Award className="w-4 h-4 inline mr-1" />
                          {feature}
                        </span>
                      ))}
                    </div>
                  </div>

                  {/* 코스 정보 */}
                  <div className="mb-8">
                    <h3 className="text-lg font-semibold text-gray-900 mb-3">🗺️ 코스 정보</h3>
                    <div className="grid md:grid-cols-2 gap-4 text-sm">
                      <div>
                        <strong>예상 날씨:</strong> {selectedMarathon.weatherExpected}<br/>
                        <strong>코스 특성:</strong> {selectedMarathon.elevation}<br/>
                        <strong>교통 통제:</strong> {selectedMarathon.trafficControl}
                      </div>
                      <div>
                        <strong>주차 안내:</strong> {selectedMarathon.parkingInfo}<br/>
                        <strong>편의시설:</strong> {selectedMarathon.facilities.join(', ')}
                      </div>
                    </div>
                  </div>

                  {/* 참가신청 방법 */}
                  <div className="mb-8">
                    <h3 className="text-lg font-semibold text-gray-900 mb-4">📝 참가신청 방법</h3>
                    <div className="space-y-4">
                      {getRegistrationGuide(selectedMarathon).map((guide, idx) => (
                        <div key={idx} className="flex items-start space-x-4 p-4 bg-gray-50 rounded-lg">
                          <div className="flex-shrink-0 w-8 h-8 bg-blue-500 text-white rounded-full flex items-center justify-center font-bold text-sm">
                            {guide.step}
                          </div>
                          <div className="flex-1">
                            <h4 className="font-medium text-gray-900">{guide.title}</h4>
                            <p className="text-sm text-gray-600 mt-1">{guide.description}</p>
                            {guide.detail && (
                              <p className="text-xs text-blue-600 mt-1">{guide.detail}</p>
                            )}
                          </div>
                        </div>
                      ))}
                    </div>
                  </div>

                  {/* 시상 및 기념품 */}
                  <div className="mb-8">
                    <h3 className="text-lg font-semibold text-gray-900 mb-3">🎁 시상 및 기념품</h3>
                    <div className="bg-purple-50 p-4 rounded-lg">
                      <div className="flex flex-wrap gap-2">
                        {selectedMarathon.awards.map((award, idx) => (
                          <span key={idx} className="px-3 py-1 bg-purple-200 text-purple-800 rounded-full text-sm">
                            {award}
                          </span>
                        ))}
                      </div>
                    </div>
                  </div>

                  {/* 액션 버튼 */}
                  <div className="flex flex-col sm:flex-row gap-4">
                    <a
                      href={selectedMarathon.website}
                      target="_blank"
                      rel="noopener noreferrer"
                      className="flex-1 bg-blue-500 text-white px-6 py-4 rounded-xl font-semibold text-center hover:bg-blue-600 transition-colors flex items-center justify-center"
                    >
                      <ExternalLink className="w-5 h-5 mr-2" />
                      공식 홈페이지에서 참가신청
                    </a>
                    
                    <button
                      onClick={() => toggleFavorite(selectedMarathon.id)}
                      className={`px-6 py-4 rounded-xl font-medium transition-colors flex items-center justify-center ${
                        favorites.includes(selectedMarathon.id)
                          ? 'bg-red-100 text-red-700 border-2 border-red-200'
                          : 'bg-gray-100 text-gray-700 border-2 border-gray-200 hover:bg-gray-200'
                      }`}
                    >
                      <Heart className={`w-5 h-5 mr-2 ${favorites.includes(selectedMarathon.id) ? 'fill-current' : ''}`} />
                      {favorites.includes(selectedMarathon.id) ? '즐겨찾기 해제' : '즐겨찾기 추가'}
                    </button>

                    <button
                      onClick={() => {
                        const mockApplicationData = {
                          name: '미누',
                          phone: '010-1234-5678',
                          birthDate: '1980-01-01'
                        };
                        addApplication(selectedMarathon.id, '풀코스', mockApplicationData);
                      }}
                      className="px-6 py-4 bg-green-500 text-white rounded-xl font-medium hover:bg-green-600 transition-colors flex items-center justify-center"
                    >
                      <CheckCircle className="w-5 h-5 mr-2" />
                      참가신청 추가
                    </button>
                  </div>

                  {/* 특별 안내 */}
                  {selectedMarathon.specialNote && (
                    <div className="mt-6 p-4 bg-blue-50 border border-blue-200 rounded-lg">
                      <div className="flex items-start">
                        <AlertCircle className="w-5 h-5 text-blue-500 mt-0.5 mr-2" />
                        <div>
                          <h4 className="font-medium text-blue-800">특별 안내</h4>
                          <p className="text-sm text-blue-700 mt-1">{selectedMarathon.specialNote}</p>
                        </div>
                      </div>
                    </div>
                  )}
                </div>
              </div>
            ) : (
              /* 대회 목록 */
              <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
                {filteredMarathons.map((marathon) => (
                  <div key={marathon.id} className="bg-white rounded-xl shadow-lg border border-gray-200 hover:shadow-xl transition-all duration-300 overflow-hidden">
                    {/* 카드 헤더 */}
                    <div className="bg-gradient-to-r from-blue-500 to-green-500 p-4 text-white">
                      <div className="flex justify-between items-start">
                        <div className="flex-1">
                          <h3 className="text-lg font-bold leading-tight mb-1">{marathon.name}</h3>
                          <p className="text-blue-100 text-sm">{marathon.organizer}</p>
                          <div className="flex items-center mt-2">
                            {getCategoryIcon(marathon.category)}
                            <span className="ml-1 text-sm">{marathon.category}</span>
                          </div>
                        </div>
                        <button
                          onClick={() => toggleFavorite(marathon.id)}
                          className="ml-2 p-1 hover:bg-white/20 rounded-full transition-colors"
                        >
                          <Heart
                            className={`w-5 h-5 ${
                              favorites.includes(marathon.id) 
                                ? 'fill-current text-red-200' 
                                : 'text-white'
                            }`}
                          />
                        </button>
                      </div>
                    </div>

                    <div className="p-4">
                      {/* 상태 및 난이도 배지 */}
                      <div className="flex justify-between items-center mb-3">
                        <span className={`px-3 py-1 rounded-full text-xs font-semibold border ${getStatusColor(marathon.status)}`}>
                          {marathon.status}
                        </span>
                        <div className="flex items-center space-x-2">
                          <span className={`px-2 py-1 rounded-full text-xs font-medium ${getDifficultyColor(marathon.difficulty)}`}>
                            {marathon.difficulty}
                          </span>
                          {marathon.recordRequired && (
                            <span className="px-2 py-1 rounded-full text-xs font-medium bg-orange-100 text-orange-800">
                              기록필수
                            </span>
                          )}
                        </div>
                      </div>

                      {/* 기본 정보 */}
                      <div className="space-y-2 mb-4">
                        <div className="flex items-center text-sm text-gray-600">
                          <Calendar className="w-4 h-4 mr-2 text-blue-500" />
                          <span>{formatDate(marathon.date)} ({getDaysUntil(marathon.date)}일 전)</span>
                        </div>
                        
                        <div className="flex items-center text-sm text-gray-600">
                          <MapPin className="w-4 h-4 mr-2 text-blue-500" />
                          <span>{marathon.region} • {marathon.location.split(' → ')[0]}</span>
                        </div>
                        
                        <div className="flex items-center text-sm text-gray-600">
                          <Users className="w-4 h-4 mr-2 text-blue-500" />
                          <span>최대 {marathon.maxParticipants.toLocaleString()}명</span>
                        </div>

                        <div className="flex items-center text-sm text-gray-600">
                          <Clock className="w-4 h-4 mr-2 text-blue-500" />
                          <span>{marathon.startTime} 출발</span>
                        </div>
                      </div>

                      {/* 종목 및 참가비 */}
                      <div className="mb-4">
                        <div className="text-xs text-gray-500 mb-1">참가종목</div>
                        <div className="flex flex-wrap gap-1 mb-2">
                          {marathon.courses.slice(0, 3).map((course, idx) => (
                            <span key={idx} className="px-2 py-1 bg-gray-100 text-gray-700 rounded text-xs">
                              {course.split('(')[0]}
                            </span>
                          ))}
                          {marathon.courses.length > 3 && (
                            <span className="px-2 py-1 bg-gray-100 text-gray-700 rounded text-xs">
                              +{marathon.courses.length - 3}
                            </span>
                          )}
                        </div>
                        
                        <div className="flex items-center justify-between">
                          <div>
                            <div className="text-xs text-gray-500">참가비</div>
                            <div className="text-sm font-semibold text-green-600">
                              {Math.min(...Object.values(marathon.fee)).toLocaleString()}원부터
                            </div>
                          </div>
                          
                          {applications.some(app => app.marathonId === marathon.id) && (
                            <div className="flex items-center text-xs text-green-600">
                              <CheckCircle className="w-4 h-4 mr-1" />
                              신청완료
                            </div>
                          )}
                        </div>
                      </div>

                      {/* 접수 일정 */}
                      <div className="mb-4 p-3 bg-gray-50 rounded-lg">
                        <div className="text-xs text-gray-500 mb-1">접수 일정</div>
                        <div className="text-xs text-gray-600">
                          신청: {new Date(marathon.registrationStart).toLocaleDateString('ko-KR')} ~ {new Date(marathon.registrationEnd).toLocaleDateString('ko-KR')}<br/>
                          결제: {new Date(marathon.paymentStart).toLocaleDateString('ko-KR')} ~ {new Date(marathon.paymentEnd).toLocaleDateString('ko-KR')}
                        </div>
                      </div>

                      {/* 액션 버튼 */}
                      <div className="flex gap-2">
                        <button
                          onClick={() => setSelectedMarathon(marathon)}
                          className="flex-1 bg-blue-500 text-white py-2 rounded-lg text-sm font-medium hover:bg-blue-600 transition-colors"
                        >
                          자세히 보기
                        </button>
                        {marathon.status === '접수중' && (
                          <a
                            href={marathon.website}
                            target="_blank"
                            rel="noopener noreferrer"
                            className="px-4 py-2 border border-blue-500 text-blue-500 rounded-lg text-sm font-medium hover:bg-blue-50 transition-colors flex items-center justify-center"
                          >
                            <ExternalLink className="w-4 h-4" />
                          </a>
                        )}
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            )}

            {/* 검색 결과 없음 */}
            {filteredMarathons.length === 0 && (
              <div className="text-center py-12">
                <div className="bg-gray-50 rounded-xl p-8 max-w-md mx-auto">
                  <Search className="w-12 h-12 text-gray-400 mx-auto mb-4" />
                  <h3 className="text-lg font-semibold text-gray-900 mb-2">검색 결과가 없습니다</h3>
                  <p className="text-gray-600">다른 검색어나 필터를 시도해보세요.</p>
                </div>
              </div>
            )}
          </>
        )}

        {/* 캘린더 뷰 */}
        {viewMode === 'calendar' && (
          <div className="bg-white rounded-xl shadow-lg p-6">
            <h2 className="text-2xl font-bold mb-6">2026년 마라톤 캘린더</h2>
            
            <div className="grid gap-4">
              {Object.entries(statistics.monthlyStats).map(([month, count]) => (
                <div key={month} className="border-l-4 border-blue-500 pl-4 py-2">
                  <div className="flex justify-between items-center">
                    <h3 className="font-semibold text-lg">{month}</h3>
                    <span className="bg-blue-100 text-blue-800 px-2 py-1 rounded-full text-sm">
                      {count}개 대회
                    </span>
                  </div>
                  <div className="mt-2 space-y-1">
                    {marathons
                      .filter(m => new Date(m.date).getMonth() === ['1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월'].indexOf(month))
                      .map(marathon => (
                        <div
                          key={marathon.id}
                          className="text-sm text-gray-600 hover:text-blue-600 cursor-pointer"
                          onClick={() => setSelectedMarathon(marathon)}
                        >
                          {new Date(marathon.date).getDate()}일 • {marathon.name} ({marathon.region})
                        </div>
                      ))
                    }
                  </div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* 참가신청 관리 뷰 */}
        {viewMode === 'applications' && (
          <div className="bg-white rounded-xl shadow-lg p-6">
            <div className="flex justify-between items-center mb-6">
              <h2 className="text-2xl font-bold">내 참가신청 관리</h2>
              <div className="text-sm text-gray-600">
                총 {applications.length}개 대회 신청
              </div>
            </div>

            {applications.length === 0 ? (
              <div className="text-center py-12">
                <CheckCircle className="w-12 h-12 text-gray-400 mx-auto mb-4" />
                <h3 className="text-lg font-semibold text-gray-900 mb-2">참가신청 내역이 없습니다</h3>
                <p className="text-gray-600 mb-4">관심 있는 대회에 참가신청을 해보세요!</p>
                <button
                  onClick={() => setViewMode('list')}
                  className="bg-blue-500 text-white px-6 py-2 rounded-lg hover:bg-blue-600 transition-colors"
                >
                  대회 둘러보기
                </button>
              </div>
            ) : (
              <div className="space-y-4">
                {applications.map((application) => {
                  const marathon = marathons.find(m => m.id === application.marathonId);
                  return (
                    <div key={application.id} className="border border-gray-200 rounded-lg p-4">
                      <div className="flex justify-between items-start">
                        <div className="flex-1">
                          <h3 className="font-semibold text-lg">{marathon?.name}</h3>
                          <p className="text-gray-600">{application.courseType}</p>
                          <div className="text-sm text-gray-500 mt-1">
                            신청일: {new Date(application.applicationDate).toLocaleDateString('ko-KR')}
                          </div>
                        </div>
                        <div className="text-right">
                          <span className={`px-3 py-1 rounded-full text-xs font-medium ${
                            application.status === '신청완료' ? 'bg-green-100 text-green-800' :
                            application.status === '결제완료' ? 'bg-blue-100 text-blue-800' :
                            'bg-yellow-100 text-yellow-800'
                          }`}>
                            {application.status}
                          </span>
                          <div className="text-sm text-gray-500 mt-1">
                            {marathon && formatDate(marathon.date)}
                          </div>
                        </div>
                      </div>
                      
                      <div className="mt-4 flex gap-2">
                        <button
                          onClick={() => updateApplicationStatus(application.id, '결제완료')}
                          className="px-3 py-1 bg-blue-500 text-white text-sm rounded hover:bg-blue-600 transition-colors"
                        >
                          결제완료로 변경
                        </button>
                        <button
                          onClick={() => removeApplication(application.id)}
                          className="px-3 py-1 bg-red-500 text-white text-sm rounded hover:bg-red-600 transition-colors"
                        >
                          신청 취소
                        </button>
                      </div>
                    </div>
                  );
                })}
              </div>
            )}
          </div>
        )}

        {/* 통계 뷰 */}
        {viewMode === 'statistics' && (
          <div className="space-y-6">
            {/* 지역별 통계 */}
            <div className="bg-white rounded-xl shadow-lg p-6">
              <h3 className="text-xl font-bold mb-4">지역별 대회 현황</h3>
              <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
                {Object.entries(statistics.regionStats).map(([region, count]) => (
                  <div key={region} className="text-center p-4 bg-gray-50 rounded-lg">
                    <div className="text-2xl font-bold text-blue-600">{count}</div>
                    <div className="text-sm text-gray-600">{region}</div>
                  </div>
                ))}
              </div>
            </div>

            {/* 월별 대회 분포 */}
            <div className="bg-white rounded-xl shadow-lg p-6">
              <h3 className="text-xl font-bold mb-4">월별 대회 분포</h3>
              <div className="space-y-3">
                {Object.entries(statistics.monthlyStats).map(([month, count]) => (
                  <div key={month} className="flex items-center">
                    <div className="w-16 text-sm font-medium">{month}</div>
                    <div className="flex-1 mx-4">
                      <div className="bg-gray-200 rounded-full h-4">
                        <div
                          className="bg-blue-500 h-4 rounded-full"
                          style={{ width: `${(count / Math.max(...Object.values(statistics.monthlyStats))) * 100}%` }}
                        ></div>
                      </div>
                    </div>
                    <div className="w-8 text-sm font-bold text-blue-600">{count}</div>
                  </div>
                ))}
              </div>
            </div>

            {/* 참가비 통계 */}
            <div className="bg-white rounded-xl shadow-lg p-6">
              <h3 className="text-xl font-bold mb-4">참가비 분석</h3>
              <div className="grid md:grid-cols-3 gap-4">
                <div className="text-center p-4 bg-green-50 rounded-lg">
                  <div className="text-2xl font-bold text-green-600">
                    {Math.min(...marathons.map(m => Math.min(...Object.values(m.fee)))).toLocaleString()}원
                  </div>
                  <div className="text-sm text-gray-600">최저 참가비</div>
                </div>
                <div className="text-center p-4 bg-blue-50 rounded-lg">
                  <div className="text-2xl font-bold text-blue-600">
                    {statistics.avgFee.toLocaleString()}원
                  </div>
                  <div className="text-sm text-gray-600">평균 참가비</div>
                </div>
                <div className="text-center p-4 bg-red-50 rounded-lg">
                  <div className="text-2xl font-bold text-red-600">
                    {Math.max(...marathons.map(m => Math.max(...Object.values(m.fee)))).toLocaleString()}원
                  </div>
                  <div className="text-sm text-gray-600">최고 참가비</div>
                </div>
              </div>
            </div>
          </div>
        )}
      </div>

      {/* 푸터 */}
      <footer className="bg-gray-900 text-white py-12 mt-16">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
            <div>
              <h3 className="text-lg font-semibold mb-4">2026 대한민국 마라톤</h3>
              <p className="text-gray-400 text-sm">
                전국 마라톤 대회의 모든 정보와 참가신청을 한 곳에서 관리하세요.
              </p>
            </div>
            
            <div>
              <h4 className="font-semibold mb-3">주요 대회</h4>
              <ul className="space-y-2 text-sm text-gray-400">
                <li>서울마라톤 (3월)</li>
                <li>대구국제마라톤 (2월)</li>
                <li>부산마라톤 (4월)</li>
                <li>제주국제마라톤 (5월)</li>
              </ul>
            </div>
            
            <div>
              <h4 className="font-semibold mb-3">서비스</h4>
              <ul className="space-y-2 text-sm text-gray-400">
                <li>대회 일정 관리</li>
                <li>참가신청 추적</li>
                <li>즐겨찾기</li>
                <li>통계 분석</li>
              </ul>
            </div>
            
            <div>
              <h4 className="font-semibold mb-3">지원</h4>
              <ul className="space-y-2 text-sm text-gray-400">
                <li>이용 가이드</li>
                <li>대회 정보 문의</li>
                <li>기술 지원</li>
                <li>피드백</li>
              </ul>
            </div>
          </div>
          
          <div className="border-t border-gray-800 mt-8 pt-8 text-center text-sm text-gray-400">
            © 2026 대한민국 마라톤. 모든 러너의 완주를 응원합니다! 🏃‍♂️
          </div>
        </div>
      </footer>
    </div>
  );
};

export default KoreaMarathon2026App;
