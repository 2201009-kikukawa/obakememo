<script>
export default {
  props: {
    outsideTotalPoint: {
        type: Number,
        default: 0
    },
    outsideDailyPoint: {
        type: Number,
        default: 0
    }
  },
  data() {
    return {
      currentDate: "",
      currentWeekday: "", // 曜日を格納するデータプロパティを追加
      pointDataManage: []
    };
  },
  created() {
    this.initPointsLocalStorage();
    this.loadPointsFromLocalStorage();
  },
  mounted() {
    this.getCurrentDate();
  },
  methods: {
    getCurrentDate() {
        const now = new Date();
        const options = { year: 'numeric', month: 'long', day: 'numeric' };
        this.currentDate = now.toLocaleDateString('ja-JP', options);

        // 曜日を取得し、データプロパティに設定
        const weekdays = ["日", "月", "火", "水", "木", "金", "土"];
        const weekdayIndex = now.getDay(); // 0（日曜日）から 6（土曜日）までの値
        this.currentWeekday = `${weekdays[weekdayIndex]}曜日`;
    },
    initPointsLocalStorage() {
        const dateTime = new Date();
        const year = dateTime.getFullYear();
        const month = dateTime.getMonth() + 1;
        const day = dateTime.getDate();
        const date = `${year}-${month}-${day}`;
        let pointDataManage = JSON.parse(localStorage.getItem("pointDataManage"));
        if (!pointDataManage) {
            //ローカルストレージの初期化
            var initPointDataManage = {
                totalPoint: 0,
                dailyPoint: 0,
                updated: date
            };
            localStorage.setItem("pointDataManage",JSON.stringify(initPointDataManage));
            return;
        }

      if (pointDataManage["updated"] !== date) {
            let pointDataManage = JSON.parse(localStorage.getItem("pointDataManage"));
            pointDataManage["updated"] = date;
            pointDataManage["dailyPoint"] = 0;
            localStorage.setItem("pointDataManage",JSON.stringify(pointDataManage));
      }
    },
    loadPointsFromLocalStorage() {
      // ローカルストレージからポイントを読み込む
      this.pointDataManage = JSON.parse(localStorage.getItem("pointDataManage"));
    },
    }
};
</script>

<template>
    <main>
        <header>
            <div class="px-3 py-2 bg-dark text-white">
                <div class="container">
                    <div class="d-flex flex-wrap align-items-center justify-content-center justify-content-lg-start">
                        <div class="d-flex align-items-center my-2 my-lg-0 me-lg-auto text-white text-decoration-none">
                            <div class="date-display text-white" style="text-align: left;">
                                合計ポイント: {{ pointDataManage['totalPoint'] + outsideTotalPoint }}<br>
                                <div style="padding-right: 30px;">デイリーポイント: {{ pointDataManage['dailyPoint'] + outsideDailyPoint }}/5</div>
                            </div>
                            <div class="date-display text-white">
                                {{ currentDate }}
                                <span class="weekday">{{ currentWeekday }}</span>
                            </div>
                        </div>

                        <ul class="nav col-12 col-lg-auto my-2 justify-content-center my-md-0 text-small">
                            <li class="Home">
                                <div class="nav-link text-secondary">
                                    <router-link to="/" style="color: rgb(255, 255, 255);">
                                    <img src="@/assets/img/Home.png" class="bi d-block mx-auto mb-1" width="45" height="30">ホーム
                                    </router-link>
                                </div>
                            </li>
                            <li class="Grave">
                                <div class="nav-link text-white">
                                    <router-link to="/GravePage" style="color: rgb(255, 255, 255);">
                                    <img src="@/assets/img/Grave.png" class="bi d-block mx-auto mb-1" width="45" height="30">墓
                                    </router-link>
                                </div>
                            </li>
                            <li class="Calendar">
                                <div class="nav-link text-white">
                                    <router-link to="CalendarPage" style="color: rgb(255, 255, 255);">
                                    <img src="@/assets/img/Calendar.png" class="bi d-block mx-auto mb-1" width="45" height="30">カレンダー
                                    </router-link>
                                </div>
                            </li>
                            <li class="Ghost">
                                <div class="nav-link text-white">
                                    <router-link to="ReleasePage" style="color: rgb(255, 255, 255);">
                                    <img src="@/assets/img/Ghost.png" class="bi d-block mx-auto mb-1" width="45" height="30">おばけ図鑑
                                    </router-link>
                                </div>
                            </li>
                            <li class="Ghost">
                                <div class="nav-link text-white">
                                    <router-link to="GhostPage" style="color: rgb(255, 255, 255);">
                                    <img src="@/assets/img/bell.png" class="bi d-block mx-auto mb-1" width="45" height="30">通知
                                    </router-link>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </header>
    </main>
</template>


<!-- スタイルを適用するためのスタイルセクションを追加 -->
<style>
.date-display {
  font-size: 16px; /* 曜日のフォントサイズを調整 */
  margin-left: 5px; /* 日付との間に少しスペースを入れる */
}
</style>