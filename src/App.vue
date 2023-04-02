<template>
	<section class="todoapp">
		<hm-header @add="handleAdd"></hm-header>
		<!-- 注意动态属性有两个作用，一个是元素自身的属性，一个是父传子自定义的属性 -->
		<hm-main
			:list="showList"
			@del="handleDel"
			@changeState="handleChangeState"
			@changeAll="changeAll"
		></hm-main>
		<hm-footer
    	:list="list"
			:type="type"
			@clear="handleClear"
			@changeType="changeType"
		></hm-footer>
	</section>
</template>

<script>
	import HmHeader from "./components/hm-header.vue";
	import HmMain from "./components/hm-main.vue";
	import HmFooter from "./components/hm-footer.vue";
	export default {
		components: {
			HmHeader,
			HmMain,
			HmFooter,
		},
		data() {
			return {
				// 这个list是原始的，完整的数据，给子组件传的数据，是过滤后的数据，用到下面的计算属性showList
				// 注意本地存储如果为空，则默认拿到null，null是不能作为list的初始值的
				list: JSON.parse(localStorage.getItem("todoList")) || [],
				// type是根据选择底部不同的条件筛选，然后反馈给main不同的值，进行不同的渲染
				type: "all", // 过滤条件
			};
		},
		computed: {
			// 给用户看的数据，基于原始数据 和 过滤条件计算得到的
			showList() {
				if (this.type === "all") {
					return this.list;
				} else if (this.type === "active") {
					// 过滤出未完成的展示
					return this.list.filter((item) => item.flag === false);
				} else {
					// 过滤出已完成的展示
					return this.list.filter((item) => item.flag === true);
				}
			},
		},
		methods: {
			handleAdd(value) {
				// 往数组的最前面，添加一项  更新视图 => 修改数据
				this.list.unshift({
					id: +new Date(),
					name: value,
					flag: false,
				});
			},
			handleDel(id) {
				// 过滤
				this.list = this.list.filter((item) => item.id !== id);
			},
			handleChangeState(flag, id) {
				// 根据 id 找到数组中的对应项，进行更新 find
				const obj = this.list.find((item) => item.id === id);
				obj.flag = flag;
			},
      changeAll(flag) {
				// 让list中的所有的任务，和flag同步
				this.list.forEach((item) => (item.flag = flag));
			},
			handleClear() {
				// 保留所有未完成的
				this.list = this.list.filter((item) => item.flag === false);
			},
			changeType(type) {
				this.type = type;
			},
		},
		watch: {
			list: {
				deep: true,
				handler(newValue) {
					// 存本地之前，先转 JSON
					localStorage.setItem("todoList", JSON.stringify(newValue));
				},
			},
		},
	};
</script>

<style></style>
