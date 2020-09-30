<script>
	import axios from 'axios'
	import { onDestroy, onMount } from "svelte";

	import Header from "./Header.svelte";
	import BookContainer from "./BookContainer.svelte";
	
	const API_ID = "iGoxXpjZyLw680L1yKT4gBT5Y2KA1wEd"
	const bookDataTypeListEndpoint = "lists/names"
	const getBooksOfType = type => `lists/current/${type}`
	let bookDataTypes = [];
	let booksAndAccordingDataType = []
	let bookCount = 0

	const fetchBooksForGivingDataType = async type => {
		const response = await axios(
				`https://api.nytimes.com/svc/books/v3/${getBooksOfType(type)}.json?api-key=${API_ID}`
			)
		booksAndAccordingDataType = [] // resetting state of array to avoid duplicate renderings
		booksAndAccordingDataType = [...booksAndAccordingDataType, response.data.results]
		bookCount = booksAndAccordingDataType[0].books.length
	}

	const clearLocalStorage = () => localStorage.clear()
	const setRefetchInterval = setInterval(clearLocalStorage, 3600000);

	onMount(async () => {
		let cachedBookDataTypes = JSON.parse(localStorage.getItem('bookDataTypes'))
		if (!!cachedBookDataTypes) {
			bookDataTypes = [...cachedBookDataTypes]
		} else {
			try {
				const response = await axios(
					`https://api.nytimes.com/svc/books/v3/${bookDataTypeListEndpoint}.json?api-key=${API_ID}`
				)
				response.data.results.forEach(type => {
					bookDataTypes = [...bookDataTypes, [type.list_name, type.list_name_encoded]].sort()
				})
				localStorage.setItem('bookDataTypes', JSON.stringify(bookDataTypes))
			} catch(e) {
				console.log(e)
			}
		}
	})

	onDestroy(() => clearInterval(setRefetchInterval))
	
</script>

<Header />
<div class="wrapper">
	<BookContainer 
		{bookDataTypes} 
		{fetchBooksForGivingDataType} 
		{booksAndAccordingDataType} 
		{bookCount}
	/>
</div>

<style>
	div.wrapper {
	}
</style>