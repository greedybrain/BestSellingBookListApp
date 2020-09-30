<script>
        export let title;
        export let author;
        export let description
        export let bookImage;

</script>

<li class="book_card">
        <!-- <h2>{title}</h2>
        <h4>{author}</h4>
        <p>{description}</p> -->
        <div class="image">
                <img height="350px" src={bookImage} alt={title}>
        </div>
</li>

<style>
        li.book_card {
                width: 250px;
                cursor: pointer;
                border: 1px solid red;
                margin: 0 auto;
        }

        li.book_card img {
                -webkit-box-shadow: 0px 0px 8px -2px rgba(0,0,0,0.75);
                -moz-box-shadow: 0px 0px 8px -2px rgba(0,0,0,0.75);
                box-shadow: 0px 0px 8px -2px rgba(0,0,0,0.75);
                width: 100%;
        }

</style>