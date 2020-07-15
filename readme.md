<h2>sib-list</h2>
基于uni-app实现列表list的上拉加载，下拉刷新，自定义列表内容<br/>
参考自: <a href="https://ext.dcloud.net.cn/plugin?id=443">https://ext.dcloud.net.cn/plugin?id=443</a>

<h2>插件预览图</h2>
<img src="https://raw.githubusercontent.com/gameruleCEO/sib-list/master/other/sib-list.gif" />

<h2>使用教程</h2>
<ul>
    <li>
        导入组件
    </li>
    
    import sibList from '@/components/sib-list/sib-list.vue'
    
</ul>
<ul>
    <li>
       声名组件
    </li>
    
    components: {
        sibList
    },
    
</ul>
<ul>
    <li>
        使用组件
    </li>
    
    <!-- @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend" 此三个方法必须写 -->
    <view class="list" @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend">
        <!-- ref="sibList" @isRefresh='isRefresh' @scrolltolowerFn="scrolltolowerFn" 此三个必须写 -->
        <sib-list ref="sibList" @isRefresh='isRefresh' @scrolltolowerFn="scrolltolowerFn">
            <!-- 内部block可自定义 -->
            <block class="" slot="sibScrollList">
                <view class="" v-for="(item, index) in list" :key="index">
                    <view class="" style="height: 100px;">{{item}}</view>
                </view>
            </block>
        </sib-list>
    </view>
    
</ul>
<ul>
    <li>
        函数实现
    </li>
    
    // 刷新touch监听
    touchstart: function(e) {
        this.$refs.sibList.refreshStart(e);
    },
    touchmove: function(e) {
        this.$refs.sibList.refreshMove(e);
    },
    touchend: function(e) {
        this.$refs.sibList.refreshEnd(e);
    },
    isRefresh: function() {
        const _this = this
        setTimeout(() => {
            uni.showToast({
                icon: 'success',
                title: '刷新成功,数据恢复初始值'
            })
            const defaultData = [
                '初始数据1', '初始数据2', '初始数据3', '初始数据4',
                '初始数据5', '初始数据6'
            ]
            _this.list = defaultData
            // 刷新结束调用
            this.$refs.sibList.endAfter()
        }, 1000)
    },
    scrolltolowerFn: function() {
        uni.showLoading({
            title: '加载中...',
            mask: true
        })
        // 模拟请求
        const _this = this
        setTimeout(() => {
            // 请求成功
            const newData = [
                '新数据1', '新数据2', '新数据3', '新数据4'
            ]
            _this.list = _this.list.concat(newData)
            console.log(_this.list)
            uni.hideLoading()
        }, 1000)
    }
    
</ul>

<h2>参数说明</h2>
<table border="1">
  <tr>
    <th>参数</th>
    <th>说明</th>
    <th>是否必填</th>
    <th>默认值</th>
  </tr>
  <tr>
    <td>isTop</td>
    <td>不必修改</td>
    <td>否</td>
    <td>1</td>
  </tr>
  <tr>
    <td>loadText</td>
    <td>下拉刷新显示文字</td>
    <td>否</td>
    <td>松开刷新</td>
  </tr>
  <tr>
    <td>scrollHeight</td>
    <td>指定滚动区域高度(可填像素或百分比,例如: 200px||20%)</td>
    <td>是</td>
    <td>500px</td>
  </tr>
  <tr>
    <td>isNoList</td>
    <td>list列表是否为空</td>
    <td>是</td>
    <td>false</td>
  </tr>
  <tr>
    <td>noListText</td>
    <td>空list列表提示文字</td>
    <td>否</td>
    <td>暂无数据...</td>
  </tr>
  <tr>
    <td>floterText</td>
    <td>底部加载区提示文字</td>
    <td>否</td>
    <td>数据加载中...</td>
  </tr>
  <tr>
    <td>isUseTop</td>
    <td>是否开启top置顶按钮</td>
    <td>否</td>
    <td>true</td>
  </tr>
  <tr>
    <td>isGtHeight</td>
    <td>距离顶部多少显示top按钮</td>
    <td>否</td>
    <td>1000</td>
  </tr>
</table>

<h2>函数事件说明</h2>
<table border="1">
  <tr>
    <th>事件</th>
    <th>说明</th>
  </tr>
  <tr>
    <td>isRefresh</td>
    <td>下拉刷新函数</td>
  </tr>
  <tr>
    <td>scrolltolowerFn</td>
    <td>滚动触底函数</td>
  </tr>
</table>

<h2>其他</h2>
如有bug请前往github更正, github地址: 
<a href="https://github.com/gameruleCEO/sib-list">https://github.com/gameruleCEO/sib-list</a>
