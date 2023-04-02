<template>
	<!-- 头部部分 -->
	<header class="header">
		<h1>todos</h1>
		<input
			v-model="todoName"
			@keyup.enter="add"
			class="new-todo"
			placeholder="请输入任务名称"
			autofocus
		/>
	</header>
</template>

<script>
	export default {
		data() {
			return {
				todoName: "",
			};
		},
		methods: {
			add() {
				if (this.todoName.trim() === "") return;
				// 通过触发事件的方式，向父组件发起修改数据的申请
				this.$emit("add", this.todoName);
				// 清空内容
				this.todoName = "";
			},
		},
	};
</script>

<style></style>
