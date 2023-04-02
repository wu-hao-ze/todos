<template>
	<!-- 底部部分 => list有数据的时候，才展示，判断list.length > 0 -->
	<footer v-if="list.length > 0" class="footer">
		<span class="todo-count">
			<strong>{{ leftCounts }}</strong>
			剩余
		</span>
		<ul class="filters">
			<li>
				<a
					@click="filter('all')"
					:class="{ selected: type === 'all' }"
					href="#/"
					>全部</a
				>
			</li>
			<li>
				<a
					@click="filter('active')"
					:class="{ selected: type === 'active' }"
					href="#/active"
					>进行中</a
				>
			</li>
			<li>
				<a
					@click="filter('completed')"
					:class="{ selected: type === 'completed' }"
					href="#/completed"
					>已完成</a
				>
			</li>
		</ul>
		<!-- 有已完成的任务，才需要显示这个按钮 -->
		<button v-if="isShowClear" @click="clear" class="clear-completed">
			清除已完成
		</button>
	</footer>
</template>

<script>
	export default {
		props: {
			list: Array,
			// 这里要注意一点！！！！！！！
			// footer和main是兄弟组件，他们用到了同一个type
			// 当同父的兄弟组件之间，如果要数据通信，可以将共享的数据进行状态提升！！！！！
			// 如果把type放在footer里定义，那么只会影响到底部，所以要放到父组件中定义该数据，这就是状态提升
			type: String,
		},
		computed: {
			// 根据数据，动态计算得到一些值 => 计算属性
			// 未完成的数量
			leftCounts() {
				return this.list.filter((item) => item.flag === false).length;
			},
			// 是否显示 - 清除已完成的按钮
			isShowClear() {
				// 只要有一个已完成，就可以显示按钮 some
				return this.list.some((item) => item.flag === true);
			},
		},
		methods: {
			clear() {
				// 子传父，子组件触发事件
				this.$emit("clear");
			},
			filter(type) {
				this.$emit("changeType", type);
			},
		},
	};
</script>

<style></style>
