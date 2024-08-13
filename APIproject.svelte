<script lang="ts">
    import {
        Email,
        Reddit,
        Pinterest,
        Telegram,
        WhatsApp,
        X
    } from 'svelte-share-buttons-component';

    let content: string = '';
    let selectedImage: File | null = null;
    const hashtags: string = 'Svelte,WebDevelopment,JavaScript';

    async function shareContent() {
        if (navigator.canShare && selectedImage && navigator.canShare({ files: [selectedImage] })) {
            try {
                const shareData: ShareData = {
                    text: content,
                    files: [selectedImage],
                };

                await navigator.share(shareData);
                console.log('content shared succesfully');
            } catch (err) {
                console.error('share failed:', err);
            }
        } else if (navigator.share) {
            try {
                const shareData: ShareData = {
                    text: content,
                };

                await navigator.share(shareData);
                console.log('content shared succesfully without image');
            } catch (err) {
                console.error('share failed:', err);
            }
        } else {
            console.log('API not supported.');
        }
    }

    function handleImageChange(event: Event) {
        const input = event.target as HTMLInputElement;
        const file = input.files ? input.files[0] : null;
        if (file) {
            selectedImage = file;
        }
    }
</script>

<style>
    .container {
        text-align: center;
        padding: 20px;
        border: 1px solid #faf2f2;
        border-radius: 7px;
        background-color: #c1e3f4;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }

    input[type="text"] {
        width: 100%;
        padding: 10px;
        margin-bottom: 20px;
        border: 1px solid #ffffff;
        border-radius: 5px;
    }

    input[type="file"] {
        display: block;
        margin: 20px auto;
    }

    button {
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        background-color: #ff00c3;
        color: white;
        cursor: pointer;
        font-size: 16px;
        margin: 5px;
    }

    button:hover {
        background-color: #0056b3;
    }

    .social-buttons {
        display: flex;
        justify-content: center;
        gap: 10px;
        flex-wrap: wrap;
    }


</style>

<div class="container">
    <h1>Share to Social Media</h1>
    <input type="text" bind:value={content} placeholder="type your text">
    <input type="file" accept="image/*" on:change={handleImageChange}>
    <button on:click={shareContent}>Share the Image</button>

    <div class="social-buttons">
        <Email subject={content} body={content} />
        <Reddit class="share-button" title={content} />
        <Pinterest class="share-button" media={selectedImage ? URL.createObjectURL(selectedImage) : "https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Svelte_Logo.svg/200px-Svelte_Logo.svg.png"} description={content} />
        <Telegram class="share-button" text={content} />
        <WhatsApp class="share-button" text={content} />
        <X class="share-button" text={`${content} #${hashtags}`} />
    </div>
</div>
