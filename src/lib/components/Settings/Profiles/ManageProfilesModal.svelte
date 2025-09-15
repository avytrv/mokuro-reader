<script lang="ts">
  import {
    copyProfile,
    createProfile,
    deleteProfile,
    profiles,
    renameProfile
  } from '$lib/settings';
  import { promptConfirmation, showSnackbar } from '$lib/util';
  import { Listgroup, ListgroupItem, Modal, Input } from 'flowbite-svelte';
  import {
    CirclePlusSolid,
    EditOutline,
    FileCopySolid,
    TrashBinSolid,
    UserEditSolid
  } from 'flowbite-svelte-icons';

  export let open = false;

  $: items = Object.keys($profiles);

  let newProfile: string;

  function onSubmit() {
    if (!newProfile) {
      showSnackbar('Profile name cannot be empty');
      return;
    }

    if (items.includes(newProfile)) {
      showSnackbar('Profile already exists');
      return;
    }

    createProfile(newProfile);
    newProfile = '';
  }

  function onCopy(item: string) {
    let newCopy = `${item} copy`;

    while (items.includes(newCopy)) {
      newCopy += ` copy`;
    }

    copyProfile(item as string, newCopy);
  }

  function onDelete(item: string) {
    promptConfirmation(`Are you sure you would like to delete the [${item}] profile?`, () => {
      deleteProfile(item as string);
    });
  }

  let profileToEdit: string;
  let newName: string;

  function onEditClicked(item: string) {
    if (profileToEdit) {
      profileToEdit = '';
    } else {
      newName = item;
      profileToEdit = item;
    }
  }

  function onEdit() {
    if (items.includes(newName as string)) {
      showSnackbar('Profile already exists');
      return;
    }

    renameProfile(profileToEdit as string, newName as string);
  }

  function onInputClick(this: any) {
    this.select();
  }
</script>

<Modal size="xs" bind:open outsideclose>
  <Listgroup>
    {#each items as item}
      <ListgroupItem class="flex flex-row justify-between gap-6">
        <div class="flex-1">
          {#if profileToEdit === item}
            <form on:submit|preventDefault={onEdit}>
              <Input size="sm" bind:value={newName} autofocus onclick={onInputClick}>
                {#snippet right()}
                  <EditOutline
                      size="sm"
                      onclick={onEdit}
                      class="hover:text-primary-700"
                    />
                {/snippet}
              </Input>
            </form>
          {:else}
            <p class="line-clamp-1">{item}</p>
          {/if}
        </div>
        <div class="flex flex-row gap-2 items-center">
          <FileCopySolid size="sm" class="hover:text-primary-700" onclick={() => onCopy(item)} />
          {#if item !== 'Default'}
            <UserEditSolid
              size="sm"
              class="hover:text-primary-700"
              onclick={() => onEditClicked(item)}
            />
            <TrashBinSolid size="sm" class="hover:text-primary-700" onclick={() => onDelete(item)} />
          {/if}
        </div>
      </ListgroupItem>
    {/each}
  </Listgroup>
  <form on:submit|preventDefault={onSubmit}>
    <Input type="text" placeholder="New profile..." bind:value={newProfile}>
      {#snippet right()}
        <CirclePlusSolid class="hover:text-primary-700" onclick={onSubmit} />
      {/snippet}
    </Input>
  </form>
</Modal>
