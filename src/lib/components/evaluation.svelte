<script lang="ts">
    import { clickOnEnter } from '$lib/helpers/a11y';

    export let value: number = null;
</script>

<fieldset class="u-margin-block-start-8">
    <legend class="label is-required u-margin-0 u-line-height-1-5 u-bold">
        <slot />
    </legend>
    <ul class="u-flex u-main-space-between u-margin-block-start-16">
        {#each Array(10) as _, i}
            <li on:keyup={clickOnEnter} on:click={() => (value = i)}>
                <input
                    type="radio"
                    name="recommend"
                    class="radio-button"
                    data-text={i}
                    checked={value === i} />
            </li>
        {/each}
    </ul>
    <div class="u-flex u-main-space-between u-margin-block-start-8">
        <span>Not at all likely</span>
        <span>Extremely likely</span>
    </div>
</fieldset>
