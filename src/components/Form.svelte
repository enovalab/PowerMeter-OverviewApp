<script>
    import { createEventDispatcher } from "svelte";
    import { correctObjectType } from "../modules/Helpers";
    import Field from "./Field.svelte";

    export let fields = [];
    export let data = {};

    const dispatchEvent = createEventDispatcher();

    function handleSubmit(event) {
        data = correctObjectType(Object.fromEntries(new FormData(event.target).entries()));
        dispatchEvent("submit", data);
    }
</script>

<form on:submit|preventDefault={handleSubmit}>
    <div class="card-padding">
        <slot name="fields-before"></slot>
        {#each fields as field}
            <Field
                key={field.key}
                label={field.label}
                type={field.type}
                required={field.required}
                pattern={field.pattern}
                step={field.step}
                value={data[field.key] ?? ""}
            />
        {/each}
        <slot name="fields-after"></slot>
    </div>
    <button type="submit">
        <slot name="submit"></slot>
    </button>
</form>

<style>
    div {
        display: grid;
        grid-row-gap: 10px;
        grid-template-columns: 1fr;
    }

    @media (min-width: 700px) {
        div {
            grid-template-columns: 300px calc(100% - 300px);
        }
    }

    button {
        width: 100%;
        height:40px;
    }
</style>