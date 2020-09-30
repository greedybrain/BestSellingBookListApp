<script>

        import Book from "./Book.svelte";
        export let bookDataTypes;
        export let booksAndAccordingDataType;
        export let fetchBooksForGivingDataType;
        export let bookCount;
        const fetchBooks = () => {
                let selectBox = document.querySelector('select.select_box')
                let selectedOption = selectBox.options[selectBox.selectedIndex].value
                if(selectedOption === "Choose Book Type") {
                        booksAndAccordingDataType = []
                } else {
                        fetchBooksForGivingDataType(selectedOption)
                }
                bookCount = !!booksAndAccordingDataType[0] ? booksAndAccordingDataType[0].books.length : null
                return bookCount
        }

        const bookListNameEqualsBookType = bookType => {
                let selectBox = document.querySelector('select.select_box')
                let selectedOption = selectBox.options[selectBox.selectedIndex].value
                return bookType === selectedOption
        }

</script>

<div class="book_container">
        <!-- svelte-ignore a11y-no-onchange -->
        <select class="select_box" on:change={fetchBooks}>
                <option value="Choose Book Type" selected>Choose Book Type</option>
                {#each bookDataTypes as type}
                         <option value={type[0]} class="book_type">
                                        {type[0]}
                        </option>
                {/each}
        </select>
        <div class="result_count">
                <h3 class="count">{ !!bookCount ? bookCount + " results" : ''}</h3>
        </div>
        <ul class="books">
                {#each booksAndAccordingDataType as bookType}
                                {#if bookListNameEqualsBookType(bookType.list_name)}
                                        {#each bookType.books as book}
                                                <Book 
                                                        title={book.title}
                                                        author={book.author}
                                                        description={book.description}
                                                        bookImage={book.book_image}
                                                />
                                        {/each}
                                {/if}
                {/each}
        </ul>
</div>

<style>
        div.book_container {
                height: 90vh;
        }
        div.book_container select.select_box {
                display: block;
                padding: 20px 10px;
                font-size: 1rem;
                background-color: #333333;
                color: #fff;
                border: none;
                /* border-radius: 5px; */
                outline: none;
                margin-bottom: 20px;
                width: 100%;
                -webkit-box-shadow: 0px 2px 4px -1px rgba(0,0,0,0.75);
                -moz-box-shadow: 0px 2px 4px -1px rgba(0,0,0,0.75);
                box-shadow: 0px 2px 4px -1px rgba(0,0,0,0.75);
        }
        ul.books {
                display: grid;
                grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
                gap: 10px;
                overflow-y: scroll;
                padding: 20px;
        }

        div.result_count {
                padding: 0 20px;
                text-align: center;
        }
</style>