<script lang="ts">
  import CloudinaryLogo from '@/components/CloudinaryLogo.svelte';
  import StepEdit from '@/components/StepEdit.svelte';
  import StepUpload from '@/components/StepUpload.svelte';
  import { imageStatus } from '@/store';
  import { ImageStatus, type Notification } from '@/types.d';
  import Toast from './components/Toast.svelte';

  let notification = { status: '', message: '', color: '', show: false };
  const showNotification = async ({ status = '', message = '', color = '' }: Notification) => {
    notification.show = true;
    notification.status = status;
    notification.message = message;
    notification.color = color;

    await new Promise((resolve) => setTimeout(resolve, 3500));
    notification.show = false;
  };
</script>

<div
  class="container m-auto grid min-h-screen w-full max-w-xl scroll-px-5 grid-cols-1 place-content-center gap-10 overflow-hidden px-5"
>
  <header class="flex justify-center">
    <h1 class="text-3xl font-bold tracking-tighter text-blue-900">
      remove<span class="text-blue-600">bg</span>
    </h1>
  </header>

  <main>
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit {showNotification} />
    {/if}
  </main>

  {#if notification.show}
    <Toast status={notification.status} message={notification.message} color={notification.color} />
  {/if}

  <footer class="flex items-center justify-center gap-x-2 font-semibold">
    Made with
    <a href="https://cloudinary.com/" target="_blank" rel="noopener noreferrer">
      <CloudinaryLogo />
    </a>
  </footer>
</div>
