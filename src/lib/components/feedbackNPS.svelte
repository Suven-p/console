<script lang="ts">
    import {
        Form,
        FormList,
        InputTextarea,
        Button,
        InputText,
        InputEmail
    } from '$lib/elements/forms';
    import { feedback } from '$lib/stores/app';
    import { addNotification } from '$lib/stores/notifications';
    import Evaluation from './evaluation.svelte';

    let value: number = null;
    let message: string;
    let email: string;
    let name: string;
    async function handleSubmit() {
        try {
            await feedback.submitFeedback('feedback-nps', message, name, email, value);
            feedback.switchType('general');
            addNotification({
                type: 'success',
                message: 'Thank you for your feedback!'
            });
        } catch (error) {
            feedback.switchType('general');
            addNotification({
                type: 'error',
                message: error.message
            });
        } finally {
            feedback.toggleFeedback();
        }
    }
</script>

<section class="drop-section">
    <header class="u-flex u-main-space-between u-gap-16">
        <h4 class="body-text-1 u-bold">How are we doing?</h4>
        <button
            type="button"
            class="button is-text is-only-icon u-margin-inline-start-auto"
            style="--button-size:1.5rem;"
            aria-label="Close Modal"
            title="Close Modal"
            on:click={() => feedback.toggleFeedback()}>
            <span class="icon-x" aria-hidden="true" />
        </button>
    </header>
    <div class="u-margin-block-start-8 u-line-height-1-5">
        At Appwrite, we value the feedback of our users. That means you! We'd love to hear what you
        think.
    </div>

    <Form onSubmit={handleSubmit}>
        <Evaluation bind:value>
            How likely are you to recommend Appwrite to a friend or colleague?
        </Evaluation>
        {#if value}
            <FormList>
                <InputTextarea
                    id="feedback"
                    placeholder="Your message here"
                    label="Message"
                    required
                    bind:value={message}
                    showLabel={false} />
                <InputText label="Name" id="name" bind:value={name} placeholder="Enter name" />
                <InputEmail label="Email" id="email" bind:value={email} placeholder="Enter email" />
            </FormList>
        {/if}
        <div class="u-flex u-main-end u-gap-16 u-margin-block-start-24">
            <Button text on:click={() => feedback.toggleFeedback()}>No thanks</Button>
            <Button disabled={!value} secondary submit>Submit</Button>
        </div>
    </Form>
</section>
