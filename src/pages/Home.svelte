<script>
    import { onMount } from 'svelte'
    
    import PostForm from '../components/PostForm.svelte'

    const baseUrl = 'https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev'
    let posts = []

    onMount(async () => {
        const res = await fetch(baseUrl + '/posts');
        posts = await res.json();
    })


    function deletePost (id) {
        event.preventDefault()
        if (confirm("Are You sure?")){
            fetch(`${baseUrl}/post/${id}`, {
            method: 'DELETE'
            })
            .then(res => {
                return res.json()
            })
            .then(() =>{
                posts = posts.filter(post => post.id !== id);
            })
        }
    }


    function addPost ({detail}) {
        posts = [detail.post, ...posts];
    }

    // function addPost (event){
    //     const post = event.detail
    //     posts = [event.detail.post, ...posts]
    // }





</script>

<style>
    .delete-btn {
		color: red !important;
	}
	.card .card-content .card-title {
		margin-bottom: 0;
    }
    .card .card-content p.timestamp {
        color: #999;
        margin-bottom: 10px;
    }

</style>
<div class="row">
    <div class="col s6">
        <PostForm on:postCreated={addPost}/>
    </div>
</div>

<div class="row">

    {#if posts.length === 0}
        <h3>Loading posts...</h3>
    {:else}
        {#each posts as post}
            <div class="col s6">
                <div class="card">
                    <div class="card-content">
                        <p class="card-title">
                            {post.title}
                        </p>
                        <p class="timestamp">{post.createdAt}</p>
                        <p>{post.body}</p>
                    </div>
                    <div class="card-action">
                        
                        <a href="#" class="delete-btn" on:click={() => deletePost(post.id)}>Delete</a>

                    </div>
                </div>
            </div>
        {/each}
    {/if}

</div>