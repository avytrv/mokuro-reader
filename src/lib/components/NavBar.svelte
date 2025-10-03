<script lang="ts">
  import { Navbar, NavBrand } from 'flowbite-svelte';
  import { UserSettingsSolid, UploadSolid, CloudArrowUpOutline } from 'flowbite-svelte-icons';
  import { afterNavigate, goto } from '$app/navigation';
  import { page } from '$app/state';
  import Settings from './Settings/Settings.svelte';
  import UploadModal from './UploadModal.svelte';
  import Icon from '$lib/assets/icon.webp';

  let settingsOpen = false;
  let uploadModalOpen = false;
  let isReader = false;

  function openSettings() {
    settingsOpen = true;
  }

  afterNavigate(() => {
    isReader = page.route.id === '/[manga]/[volume]';

    if (isReader) {
      window.document.body.classList.add('reader');
    } else {
      window.document.body.classList.remove('reader');
    }
  });
</script>

<div class="relative z-10">
  <Navbar hidden={isReader} class="dark:bg-gray-900 dark:text-gray-200 dark:border-gray-700 dark:divide-gray-700">
    <NavBrand href="/">
      <div class="flex flex-row gap-2 items-center">
        <img src={Icon} alt="icon" class="w-[32px] h-[32px]" />
        <span class="text-xl font-semibold dark:text-white">Mokuro</span>
      </div>
    </NavBrand>
    <div class="flex md:order-2 gap-3">
      <UserSettingsSolid size="lg" class="hover:text-primary-700" onclick={openSettings} />
      <UploadSolid size="lg" class="hover:text-primary-700" onclick={() => (uploadModalOpen = true)} />
      <CloudArrowUpOutline size="lg" class="hover:text-primary-700" onclick={() => goto('/cloud')} />
    </div>
  </Navbar>
</div>

<Settings bind:open={settingsOpen} />
<UploadModal bind:open={uploadModalOpen} />
