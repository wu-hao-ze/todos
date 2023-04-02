<template>
	<!-- 主体部分 -->
	<section class="main">
		<!-- 全选反选按钮 -->
		<input v-model="isAll" id="toggle-all" class="toggle-all" type="checkbox" />
		<label for="toggle-all"></label>
		<ul class="todo-list">
			<!-- 当任务已完成，可以给li加上completed类，会让元素加上删除线 -->
			<li v-for="item in list" :key="item.id" :class="{ completed: item.flag }">
				<div class="view">
					<!-- 
            v-model语法糖，自动根据不同元素，设置不同的属性，监听不同的事件(vue内部) 
               (1) input type="text" 文本框
                   v-model       => :value  +  @input
                   v-model.lazy  => :value  +  @change
               (2) input text="checkbox" 复选框
                   v-model       => :checked +  @change
          -->
          <!-- 注意这里如果给checkbox直接v-model="item.flag"是不对的，因为item.flag是父组件的数据 -->
          <!-- 子组件不能直接修改父组件传过来的数据，注意因为这里是对象的一个属性，所以vue没有报错 -->
          <!-- 结论：v-model永远不要和父组件(props)的数据 直接绑定 => 没有遵循单项数据流 -->
					<input
						:checked="item.flag"
						@change="handleChange($event, item.id)"
						class="toggle"
						type="checkbox"
					/>
					<label>{{ item.name }}</label>
					<button @click="del(item.id)" class="destroy"></button>
				</div>
			</li>
		</ul>
	</section>
</template>

<script>
	export default {
		props: {
			list: {
				type: Array,
				required: true,
			},
		},
		computed: {
			isAll: {
				get() {
					// 必须所有的任务都已完成，才选中，考虑使用every
					return this.list.every((item) => item.flag === true);
				},
				set(value) {
					this.$emit("changeAll", value);
				},
			},
		},
		methods: {
			del(id) {
				this.$emit("del", id);
			},
			handleChange(e, id) {
				this.$emit("changeState", e.target.checked, id); // e.target 当前的事件源
			},
		},
	};
</script>

<style></style>
