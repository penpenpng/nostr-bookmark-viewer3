<script lang="ts">
    import { tabSet, tags } from "$lib/store";
    import {
       
        modalStore,

    } from "@skeletonlabs/skeleton";
    import { nip19 } from "nostr-tools";

    // Props
    /** Exposes parent props to this component. */
    export let parent: any;
    // Local
    let menuItem: { name: string; value?: string } = { name: "copy" };
    let isMoveListOpen = false;
    let isClickedDelete = false;
    let selectTag: string;
    $: moveList = $tags.filter((item) => item !== $tags[$tabSet]);
    // Handle Form Submission
    function onFormSubmit(): void {
        isMoveListOpen = false;
        if ($modalStore[0].response) {
            $modalStore[0].response(menuItem);
        }
        modalStore.close();
    }

    // Base Classes
    const cBase = "card p-4 w-modal shadow-xl space-y-4 break-all";
    const cHeader = "text-2xl font-bold";

    function onMoveClick() {
        isMoveListOpen = !isMoveListOpen;
        isClickedDelete = false;

        //  console.log(menuItem);
    }
    function onDeleteClick() {
        isClickedDelete = !isClickedDelete;
        isMoveListOpen = false;
    }

    function onClickMoveTag(li: string) {
        //console.log(li)
        menuItem = { name: "move", value: li };
        onFormSubmit();
    }
</script>

<!-- @component This example creates a simple form modal. -->

{#if $modalStore[0]}
    <div class="modal-example-form {cBase}">
        <header class={cHeader}>
            {$modalStore[0].title ?? "(title missing)"}
        </header>
        <article class="body">{$modalStore[0].body ?? "(body missing)"}</article>
        <div
            class="btn-group-vertical variant-filled w-full flex justify-center"
        >
            <!-- <button
                on:click={() => {
                    menuItem = "copy";
                    onFormSubmit();
                }}><strong>copy </strong>&nbsp; to clipboard</button
            > -->
            <button
                on:click={() => {
                    menuItem = {
                        name: "copy",
                    };
                    onFormSubmit();
                }}><strong>copy </strong>&nbsp; to clipboard</button
            >
            <button
                on:click={() => {
                    menuItem = { name: "open" };
                    onFormSubmit();
                }}><strong>open </strong> &nbsp; in another app</button
            >
            <button
                on:click={() => {
                    menuItem = { name: "move" };
                    onMoveClick();
                }}><strong>move </strong> &nbsp; to another tag</button
            >

            {#if isMoveListOpen}
                <select
                    class="select text-black dark:text-white"
                    size="3"
                    value="0"
                >
                <option disabled selected value="0">
                   (移動先のタグをクリックしてください)</option
                >
                    {#each moveList as li}
                        <option value="li" on:click={() => onClickMoveTag(li)}>
                            {li}</option
                        >
                    {/each}
                </select>
            {/if}
            <button
                on:click={() => {
                    menuItem = { name: "delete" };
                    onDeleteClick();
                }}
                ><strong>delete </strong> &nbsp; from <u>{$tags[$tabSet]}</u> &nbsp;
                tag</button
            >

            {#if isClickedDelete}
                <option
                    class="text-black dark:text-white btn variant-filled-warning"
                    value="delete"
                    on:click={onFormSubmit}
                >
                    Are you sure you want to delete this note?</option
                >
            {/if}
            <button on:click={parent.onClose}>{parent.buttonTextCancel}</button>
        </div>
        <!-- <ListBox class="border border-surface-500 p-4 rounded-container-token">
			<ListBoxItem bind:group={menuItem} name="copy" value="copy"><strong>copy</strong> to clipboard</ListBoxItem>
			<ListBoxItem bind:group={menuItem} name="open" value="open"><strong>open</strong> in another app</ListBoxItem>
			<ListBoxItem bind:group={menuItem} name="move" value="move"><strong>move</strong> to another tag</ListBoxItem>
			<ListBoxItem bind:group={menuItem} name="delete" value="delete"><strong>delete</strong> from <u>{$tags[$tabSet]}</u> tag</ListBoxItem>
		</ListBox> -->
        <!-- prettier-ignore -->
        <!-- <footer class="modal-footer {parent.regionFooter}">
        <button class="btn {parent.buttonNeutral}" on:click={parent.onClose}>{parent.buttonTextCancel}</button>
        <button class="btn {parent.buttonPositive}" on:click={onFormSubmit}>Select</button>
    </footer> -->
    </div>
{/if}

<style>

    .body{
        white-space: pre-wrap; word-wrap: break-word; 
    }
</style>