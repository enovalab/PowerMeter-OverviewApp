<script>
    import Form from "./Form.svelte";
    import ExpandableCard from "./ExpandableCard.svelte";
    import { createEventDispatcher } from "svelte";
    import { ipPattern } from "../modules/Helpers";
    
    let data = {};
    let isExpanded = false;

    const dispatchEvent = createEventDispatcher();

    function handleSubmit(event) {
        dispatchEvent("add", event.detail)
        isExpanded = false;
        data = {};
    }
</script>

<ExpandableCard showExpandIcon={false} bind:isExpanded={isExpanded}>
    <span slot="preview" class="material-icons-round">add</span>
    <svelte:fragment slot="content">
        <Form on:submit={handleSubmit} data={data} fields={[
            {
                key: "name",
                type: "text",
                label: "Name",
                required: true
            },
            {
                key: "ip",
                type: "text",
                label: "IP",
                required: true,
                pattern: ipPattern.source
            }
        ]}>
            <span slot="submit" class="material-icons-round">done</span>
        </Form>
    </svelte:fragment>
</ExpandableCard>

<style>
    span[slot="preview"] {
        font-size: 45px;
    }
</style>
