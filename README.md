# korean-marathon-app
마라톤 일정 수립
[marathon-korea-app (5).html](https://github.com/user-attachments/files/25197424/marathon-korea-app.5.html)
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>마라톤 코리아 - 대한민국 마라톤 대회 종합 안내</title>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://unpkg.com/lucide-react@latest/dist/umd/lucide-react.js"></script>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <style>
        .gradient-bg {
            background: linear-gradient(135deg, #3b82f6 0%, #10b981 100%);
        }
        .card-hover:hover {
            transform: translateY(-2px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        .transition-all {
            transition: all 0.3s ease;
        }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;
        const { Calendar, MapPin, Clock, Users, ExternalLink, Filter, Heart, Search, Star, Award, Route } = lucideReact;

        // 실제 마라톤 대회 데이터 (2025-2026년)
        const marathonData = [
          {
            id: 1,
            name: "2026 서울마라톤",
            organizer: "동아일보",
            date: "2026-03-22",
            location: "서울 잠실종합운동장",
            courses: ["풀코스(42.195km)", "10km"],
            registrationStart: "2025-12-01",
            registrationEnd: "2026-01-31",
            fee: { full: 60000, half: 40000, "10k": 25000 },
            status: "접수중",
            website: "https://seoul-marathon.com",
            features: ["국제공인코스", "플래티넘 라벨", "완주메달"],
            maxParticipants: 40000,
            difficulty: "중급",
            category: "메이저대회"
          },
          {
            id: 2,
            name: "2026 춘천마라톤",
            organizer: "조선일보",
            date: "2026-10-26",
            location: "강원 춘천시 공지천교",
            courses: ["풀코스(42.195km)", "10km"],
            registrationStart: "2025-06-24",
            registrationEnd: "2025-07-09",
            fee: { full: 55000, "10k": 22000 },
            status: "접수예정",
            website: "https://chuncheonmarathon.com",
            features: ["의암호 순환코스", "가을 단풍", "국제공인코스"],
            maxParticipants: 15000,
            difficulty: "초급",
            category: "풍경대회"
          },
          {
            id: 3,
            name: "2026 JTBC 서울마라톤",
            organizer: "JTBC",
            date: "2026-05-18",
            location: "서울 여의도한강공원",
            courses: ["하프(21.0975km)", "10km", "5km"],
            registrationStart: "2025-11-15",
            registrationEnd: "2026-03-30",
            fee: { half: 45000, "10k": 28000, "5k": 18000 },
            status: "접수중",
            website: "https://marathon.jtbc.com",
            features: ["한강뷰", "방송중계", "셀러브리티 참가"],
            maxParticipants: 25000,
            difficulty: "초급",
            category: "엔터테인먼트"
          },
          {
            id: 4,
            name: "2026 부산국제마라톤",
            organizer: "부산광역시",
            date: "2026-04-12",
            location: "부산 해운대해수욕장",
            courses: ["풀코스(42.195km)", "하프(21.0975km)", "10km"],
            registrationStart: "2025-12-01",
            registrationEnd: "2026-02-28",
            fee: { full: 50000, half: 35000, "10k": 20000 },
            status: "접수예정",
            website: "https://busanmarathon.com",
            features: ["해안코스", "국제대회", "관광연계"],
            maxParticipants: 30000,
            difficulty: "중급",
            category: "해안대회"
          },
          {
            id: 5,
            name: "2025 용인마라톤",
            organizer: "용인시",
            date: "2025-10-19",
            location: "경기 용인시청 앞 광장",
            courses: ["하프(21.0975km)", "10km", "5km"],
            registrationStart: "2025-08-01",
            registrationEnd: "2025-09-30",
            fee: { half: 30000, "10k": 20000, "5k": 15000 },
            status: "접수마감",
            website: "https://yonginmarathon.net",
            features: ["가족참여", "셔틀버스", "온라인 기록증명"],
            maxParticipants: 8000,
            difficulty: "초급",
            category: "지역대회"
          },
          {
            id: 6,
            name: "2025 YTN 서울투어 마라톤",
            organizer: "YTN",
            date: "2025-11-23",
            location: "서울 상암 월드컵공원",
            courses: ["하프(21.0975km)", "10km", "5km"],
            registrationStart: "2025-07-28",
            registrationEnd: "2025-09-30",
            fee: { half: 40000, "10k": 25000, "5k": 18000 },
            status: "접수중",
            website: "https://run.ytn.co.kr",
            features: ["관광코스", "방송연계", "기념품 풍부"],
            maxParticipants: 12000,
            difficulty: "초급",
            category: "관광대회"
          }
        ];

        const KoreanMarathonApp = () => {
          const [marathons, setMarathons] = useState(marathonData);
          const [filteredMarathons, setFilteredMarathons] = useState(marathonData);
          const [searchTerm, setSearchTerm] = useState('');
          const [filterStatus, setFilterStatus] = useState('전체');
          const [filterRegion, setFilterRegion] = useState('전체');
          const [filterDistance, setFilterDistance] = useState('전체');
          const [selectedMarathon, setSelectedMarathon] = useState(null);
          const [favorites, setFavorites] = useState([]);
          const [showFilters, setShowFilters] = useState(false);

          // 필터링 로직
          useEffect(() => {
            let filtered = marathons;

            if (searchTerm) {
              filtered = filtered.filter(marathon =>
                marathon.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
                marathon.location.toLowerCase().includes(searchTerm.toLowerCase())
              );
            }

            if (filterStatus !== '전체') {
              filtered = filtered.filter(marathon => marathon.status === filterStatus);
            }

            if (filterRegion !== '전체') {
              filtered = filtered.filter(marathon =>
                marathon.location.includes(filterRegion)
              );
            }

            if (filterDistance !== '전체') {
              filtered = filtered.filter(marathon =>
                marathon.courses.some(course => course.includes(filterDistance))
              );
            }

            setFilteredMarathons(filtered);
          }, [searchTerm, filterStatus, filterRegion, filterDistance, marathons]);

          // 즐겨찾기 토글
          const toggleFavorite = (marathonId) => {
            setFavorites(prev =>
              prev.includes(marathonId)
                ? prev.filter(id => id !== marathonId)
                : [...prev, marathonId]
            );
          };

          // 참가신청 방법 안내
          const getRegistrationGuide = (marathon) => {
            const steps = [
              {
                step: 1,
                title: "대회 홈페이지 접속",
                description: `${marathon.website}로 접속하여 대회 정보를 확인합니다.`
              },
              {
                step: 2,
                title: "참가신청 버튼 클릭",
                description: "메인 페이지에서 '참가신청' 또는 '접수하기' 버튼을 찾아 클릭합니다."
              },
              {
                step: 3,
                title: "개인정보 입력",
                description: "이름, 생년월일, 연락처, 주소 등 필수 정보를 정확히 입력합니다."
              },
              {
                step: 4,
                title: "종목 선택",
                description: `참가하고 싶은 종목을 선택합니다. (${marathon.courses.join(', ')})`
              },
              {
                step: 5,
                title: "참가비 결제",
                description: `신용카드, 계좌이체 등으로 참가비를 결제합니다. (${Object.entries(marathon.fee).map(([k, v]) => `${k}: ${v.toLocaleString()}원`).join(', ')})`
              },
              {
                step: 6,
                title: "접수 확인",
                description: "결제 완료 후 이메일 또는 문자로 접수 확인 메시지를 받습니다."
              }
            ];

            return steps;
          };

          const getDifficultyColor = (difficulty) => {
            switch (difficulty) {
              case '초급': return 'bg-green-100 text-green-800';
              case '중급': return 'bg-yellow-100 text-yellow-800';
              case '고급': return 'bg-red-100 text-red-800';
              default: return 'bg-gray-100 text-gray-800';
            }
          };

          const getStatusColor = (status) => {
            switch (status) {
              case '접수중': return 'bg-blue-100 text-blue-800 border-blue-200';
              case '접수예정': return 'bg-orange-100 text-orange-800 border-orange-200';
              case '접수마감': return 'bg-gray-100 text-gray-800 border-gray-200';
              default: return 'bg-gray-100 text-gray-800 border-gray-200';
            }
          };

          return (
            <div className="min-h-screen bg-gradient-to-br from-blue-50 via-white to-green-50">
              {/* 헤더 */}
              <header className="bg-white shadow-lg border-b-4 border-blue-500">
                <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
                  <div className="flex items-center justify-between">
                    <div className="flex items-center space-x-3">
                      <div className="gradient-bg p-3 rounded-full">
                        <Route className="w-8 h-8 text-white" />
                      </div>
                      <div>
                        <h1 className="text-3xl font-bold text-gray-900">마라톤 코리아</h1>
                        <p className="text-gray-600 text-sm">대한민국 마라톤 대회 종합 안내</p>
                      </div>
                    </div>
                    <div className="flex items-center space-x-4">
                      <div className="bg-blue-50 px-4 py-2 rounded-full">
                        <span className="text-blue-700 font-semibold">{filteredMarathons.length}개 대회</span>
                      </div>
                    </div>
                  </div>
                </div>
              </header>

              <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
                {/* 검색 및 필터 */}
                <div className="mb-8">
                  <div className="flex flex-col sm:flex-row gap-4 mb-4">
                    {/* 검색바 */}
                    <div className="relative flex-1">
                      <Search className="absolute left-3 top-3 w-5 h-5 text-gray-400" />
                      <input
                        type="text"
                        placeholder="대회명 또는 지역으로 검색..."
                        className="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-blue-500 text-lg"
                        value={searchTerm}
                        onChange={(e) => setSearchTerm(e.target.value)}
                      />
                    </div>
                    
                    {/* 필터 버튼 */}
                    <button
                      onClick={() => setShowFilters(!showFilters)}
                      className="flex items-center px-6 py-3 bg-blue-500 text-white rounded-xl hover:bg-blue-600 transition-colors"
                    >
                      <Filter className="w-5 h-5 mr-2" />
                      필터
                    </button>
                  </div>

                  {/* 필터 옵션 */}
                  {showFilters && (
                    <div className="bg-white p-6 rounded-xl shadow-lg border border-gray-200 mb-6">
                      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
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
                            <option value="경기">경기</option>
                            <option value="강원">강원</option>
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
                      </div>
                    </div>
                  )}
                </div>

                {/* 대회 목록 */}
                {selectedMarathon ? (
                  /* 상세보기 */
                  <div className="bg-white rounded-xl shadow-lg border border-gray-200 overflow-hidden">
                    <div className="gradient-bg p-6 text-white">
                      <button
                        onClick={() => setSelectedMarathon(null)}
                        className="mb-4 text-white hover:text-blue-200 flex items-center"
                      >
                        ← 뒤로가기
                      </button>
                      <h2 className="text-3xl font-bold">{selectedMarathon.name}</h2>
                      <p className="text-blue-100 text-lg">{selectedMarathon.organizer}</p>
                    </div>

                    <div className="p-6">
                      {/* 기본 정보 */}
                      <div className="grid md:grid-cols-2 gap-6 mb-8">
                        <div className="space-y-4">
                          <div className="flex items-center text-gray-600">
                            <Calendar className="w-5 h-5 mr-3 text-blue-500" />
                            <span className="font-medium">대회일자:</span>
                            <span className="ml-2">{new Date(selectedMarathon.date).toLocaleDateString('ko-KR', {
                              year: 'numeric',
                              month: 'long',
                              day: 'numeric',
                              weekday: 'long'
                            })}</span>
                          </div>
                          
                          <div className="flex items-center text-gray-600">
                            <MapPin className="w-5 h-5 mr-3 text-blue-500" />
                            <span className="font-medium">장소:</span>
                            <span className="ml-2">{selectedMarathon.location}</span>
                          </div>
                          
                          <div className="flex items-center text-gray-600">
                            <Users className="w-5 h-5 mr-3 text-blue-500" />
                            <span className="font-medium">참가정원:</span>
                            <span className="ml-2">{selectedMarathon.maxParticipants?.toLocaleString()}명</span>
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
                                <div key={type} className="text-sm">
                                  <span className="text-gray-600">{type}:</span>
                                  <span className="ml-2 font-semibold text-green-600">{price.toLocaleString()}원</span>
                                </div>
                              ))}
                            </div>
                          </div>
                        </div>
                      </div>

                      {/* 특징 */}
                      <div className="mb-8">
                        <h3 className="text-lg font-semibold text-gray-900 mb-3">대회 특징</h3>
                        <div className="flex flex-wrap gap-2">
                          {selectedMarathon.features.map((feature, idx) => (
                            <span key={idx} className="px-3 py-2 bg-green-50 text-green-700 rounded-lg text-sm border border-green-200">
                              <Award className="w-4 h-4 inline mr-1" />
                              {feature}
                            </span>
                          ))}
                        </div>
                      </div>

                      {/* 참가신청 방법 */}
                      <div className="mb-8">
                        <h3 className="text-lg font-semibold text-gray-900 mb-4">📝 참가신청 방법</h3>
                        <div className="space-y-4">
                          {getRegistrationGuide(selectedMarathon).map((guide, idx) => (
                            <div key={idx} className="flex items-start space-x-4 p-4 bg-gray-50 rounded-lg">
                              <div className="flex-shrink-0 w-8 h-8 bg-blue-500 text-white rounded-full flex items-center justify-center font-bold">
                                {guide.step}
                              </div>
                              <div>
                                <h4 className="font-medium text-gray-900">{guide.title}</h4>
                                <p className="text-sm text-gray-600 mt-1">{guide.description}</p>
                              </div>
                            </div>
                          ))}
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
                      </div>
                    </div>
                  </div>
                ) : (
                  /* 대회 목록 */
                  <div className="grid gap-6 md:grid-cols-2 lg:grid-cols-3">
                    {filteredMarathons.map((marathon) => (
                      <div key={marathon.id} className="bg-white rounded-xl shadow-lg border border-gray-200 hover:shadow-xl transition-all duration-300 overflow-hidden card-hover">
                        {/* 카드 헤더 */}
                        <div className="gradient-bg p-4 text-white">
                          <div className="flex justify-between items-start">
                            <div className="flex-1">
                              <h3 className="text-lg font-bold leading-tight">{marathon.name}</h3>
                              <p className="text-blue-100 text-sm">{marathon.organizer}</p>
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
                          {/* 상태 배지 */}
                          <div className="flex justify-between items-center mb-3">
                            <span className={`px-3 py-1 rounded-full text-xs font-semibold border ${getStatusColor(marathon.status)}`}>
                              {marathon.status}
                            </span>
                            <span className={`px-2 py-1 rounded-full text-xs font-medium ${getDifficultyColor(marathon.difficulty)}`}>
                              {marathon.difficulty}
                            </span>
                          </div>

                          {/* 기본 정보 */}
                          <div className="space-y-2 mb-4">
                            <div className="flex items-center text-sm text-gray-600">
                              <Calendar className="w-4 h-4 mr-2 text-blue-500" />
                              {new Date(marathon.date).toLocaleDateString('ko-KR', {
                                month: 'long',
                                day: 'numeric',
                                weekday: 'short'
                              })}
                            </div>
                            
                            <div className="flex items-center text-sm text-gray-600">
                              <MapPin className="w-4 h-4 mr-2 text-blue-500" />
                              {marathon.location}
                            </div>
                            
                            <div className="flex items-center text-sm text-gray-600">
                              <Users className="w-4 h-4 mr-2 text-blue-500" />
                              최대 {marathon.maxParticipants?.toLocaleString()}명
                            </div>
                          </div>

                          {/* 종목 */}
                          <div className="mb-4">
                            <div className="text-xs text-gray-500 mb-1">참가종목</div>
                            <div className="flex flex-wrap gap-1">
                              {marathon.courses.slice(0, 2).map((course, idx) => (
                                <span key={idx} className="px-2 py-1 bg-gray-100 text-gray-700 rounded text-xs">
                                  {course.split('(')[0]}
                                </span>
                              ))}
                              {marathon.courses.length > 2 && (
                                <span className="px-2 py-1 bg-gray-100 text-gray-700 rounded text-xs">
                                  +{marathon.courses.length - 2}
                                </span>
                              )}
                            </div>
                          </div>

                          {/* 참가비 */}
                          <div className="mb-4">
                            <div className="text-xs text-gray-500 mb-1">참가비</div>
                            <div className="text-sm font-semibold text-green-600">
                              {Math.min(...Object.values(marathon.fee)).toLocaleString()}원부터
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
                                className="px-4 py-2 border border-blue-500 text-blue-500 rounded-lg text-sm font-medium hover:bg-blue-50 transition-colors flex items-center"
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
              </div>

              {/* 푸터 */}
              <footer className="bg-gray-900 text-white py-12 mt-16">
                <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                  <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
                    <div>
                      <h3 className="text-lg font-semibold mb-4">마라톤 코리아</h3>
                      <p className="text-gray-400 text-sm">
                        대한민국 전국 마라톤 대회 정보를 한 곳에서 확인하고 참가신청하세요.
                      </p>
                    </div>
                    
                    <div>
                      <h4 className="font-semibold mb-3">주요 대회</h4>
                      <ul className="space-y-2 text-sm text-gray-400">
                        <li>서울마라톤</li>
                        <li>부산국제마라톤</li>
                        <li>춘천마라톤</li>
                        <li>JTBC 서울마라톤</li>
                      </ul>
                    </div>
                    
                    <div>
                      <h4 className="font-semibold mb-3">참가 가이드</h4>
                      <ul className="space-y-2 text-sm text-gray-400">
                        <li>초보자 가이드</li>
                        <li>훈련 방법</li>
                        <li>장비 추천</li>
                        <li>영양 관리</li>
                      </ul>
                    </div>
                    
                    <div>
                      <h4 className="font-semibold mb-3">고객 지원</h4>
                      <ul className="space-y-2 text-sm text-gray-400">
                        <li>자주 묻는 질문</li>
                        <li>대회 등록 문의</li>
                        <li>기술 지원</li>
                        <li>피드백</li>
                      </ul>
                    </div>
                  </div>
                  
                  <div className="border-t border-gray-800 mt-8 pt-8 text-center text-sm text-gray-400">
                    © 2025 마라톤 코리아. 건강한 러닝 라이프를 응원합니다. 🏃‍♂️
                  </div>
                </div>
              </footer>
            </div>
          );
        };

        ReactDOM.render(<KoreanMarathonApp />, document.getElementById('root'));
    </script>
</body>
</html>
