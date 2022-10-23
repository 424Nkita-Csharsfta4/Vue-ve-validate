<script setup lang = "ts">
import {reactive,ref} from 'vue'
import type { IPost } from './catalog.interface';
import { useField } from 'vee-validate';
import { defineProps, toRef } from 'vue';


const props = defineProps<{
   title:string
}>()



const isRequired = (value: unknown)=> 
!! (value &&( value as string).trim()) || 'This is required'
// make sure to convert the name prop to a ref to maintain its reactivity
// this way vee-validate can react to the field name changing
const titleRef = toRef(props, 'title');
const { errorMessage, value } = useField<string>(titleRef, isRequired);


const state:{posts: IPost[]} = reactive({
	posts:[
	{
		    id:1,
			userId:1,
            title:"qui est esse",
			body:"est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla",
	}
]})

const removePost = (id:number) =>{
	state.posts = 
	state.posts.filter(post =>post.id !== id)
}

const AddPost = () =>{
	state.posts.push({
             id:2,
			 title:value.value,
			 userId:2,
			 body:''
	})
	value.value=''
}
</script>

<template>
	<div>
	<h1>Katolog</h1>
	<form>
		<input v-model="value" >
		<div v-if='errorMessage'>{{errorMessage}}</div>
		<button @click.prevent="AddPost">Create</button>
	</form>
		<li  :key="post.id" v-for="post in state.posts">
			<span>{{post.id}} - {{post.body}}</span>
		    <button @click="removePost(post.id)">Remove</button>
	</li>
	</div>
	
</template>