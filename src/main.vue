<template>
	<!-- App -->
	<div id="app">
		<!-- Statusbar -->
		<f7-statusbar></f7-statusbar>
		
		<!-- Main Views -->
		<f7-views>
			<f7-view id="main-view" navbar-through :dynamic-navbar="true" main>
				<!-- Navbar -->
				<f7-navbar class="title-bar">
					<f7-nav-center sliding>看天气</f7-nav-center>
					<f7-nav-right>
						<f7-button open-popup="#popup">+</f7-button>
					</f7-nav-right>
				</f7-navbar>
				<!-- Pages -->
				<f7-pages>
					<f7-page>
						<f7-list>
              <li class="swipeout" @swipeout:deleted="onSwipeoutDeleted">
                <div class="swipeout-content item-content">
                  <div class="item-inner">
                    <div class="item-title">
                      北京 39   &#176;C
                      <p>
                        微风 有雨
                      </p>
                    </div>
                    <div class="item-after">
                        List element label
                    </div>
                  </div>
                </div>
                <div class="swipeout-actions-right">
                  <a href="#" class="swipeout-delete">
                    删除
                  </a>
                </div>
              </li>
						</f7-list>
					</f7-page>
				</f7-pages>
			</f7-view>
		</f7-views>
		
		<!-- Popup -->
		<f7-popup id="popup">
			<f7-view navbar-fixed>
				<f7-pages>
					<f7-page>
						<f7-navbar class="title-bar">
              <f7-searchbar search-list="#search-list" placeholder="请输入城市"></f7-searchbar>
							<f7-nav-right>
								<f7-link :close-popup="true">取消</f7-link>
							</f7-nav-right>
						</f7-navbar>


            <f7-block class="searchbar-not-found">
              未找到该城市
            </f7-block>

            <f7-list
              id="search-list"
              class="searchbar-found"
              v-show="items.length"
              virtual
              :virtual-items="items"
              :virtual-search-all="searchAll"
              >
              <!-- Templte 7 Virtual List Item Template -->
              <t7-template>
                <f7-list-item :title="'{{title}}'"></f7-list-item>
              </t7-template>
            </f7-list>


					</f7-page>
				</f7-pages>
			</f7-view>
		</f7-popup>
	</div>
</template>

<script>
  const crypto = require('crypto')
  const $ = require('jquery')

  // https://api.seniverse.com/v3/weather/now.json?key=spubq0leuz0kccjg&location=beijing&language=zh-Hans&unit=c

  function buildQuery (params) {
    var key = 'spubq0leuz0kccjg'
    var key = 'secret'
    var uid = 'U0D6A7CE2F'
    params.uid = uid
    params.ttl = 30
    params.ts = +new Date

    var keys = Object.keys(params).sort()
    var paramsArr = []

    for (var k of keys) {
      paramsArr.push(`${k}=${params[k]}`)
    }

    console.log(paramsArr)
    var hmac = crypto.createHmac('sha1', key)
    // hmac.update(paramsArr.join('&'));
    hmac.update('ts=1443079775&ttl=30&uid=U123456789')
    var sig = encodeURIComponent(hmac.digest().toString('base64'))
    console.log(sig)
    paramsArr.push(`sig=${sig}`)
    console.log(paramsArr.join('&'))
    var url = 'https://api.seniverse.com/v3/weather/now.json?' + paramsArr.join('&')
    console.log(url)
    $.ajax({
      url 
    }).done(data => {
      alert(data)
    })
  }

  // location=beijing&ts=1443079775&ttl=30&uid=[your_uid]&sig=[your_signature]&callback=showWeather
  buildQuery({
    location: 'beijing',
  })

	export default {
    data () {
      var items = []
      items.push({
        title: '北京',
        subtitle: '北京'
      })
      return {
        items
      }
    },
    methods: {
      searchAll (query) {
        var self = this;
        var found = [];
        for (var i = 0; i < self.items.length; i++) {
            if (self.items[i].title.indexOf(query) >= 0 || query.trim() === '') found.push(i);
        }
        return found;
      },
      onSwipeoutDeleted () {
      }
    }
  }
</script>
<style lang="sass">
.title-bar {
  color: #fff;
  .link,.button {
    color: #fff;
  }
  .button {
    border-color: #fff;
  }
  background: rgba(27, 22, 47, 0.88);
  .searchbar {
    background: none;
  }
}
</style>
