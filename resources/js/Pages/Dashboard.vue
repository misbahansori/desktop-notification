<template>
  <breeze-authenticated-layout>
    <template #header>
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">Dashboard</h2>
    </template>

    <div class="py-12">
      <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">
          <div class="p-6 bg-white border-b border-gray-200">You're logged in!</div>
        </div>
      </div>
    </div>
  </breeze-authenticated-layout>
</template>

<script>
import BreezeAuthenticatedLayout from "@/Layouts/Authenticated";

export default {
  components: {
    BreezeAuthenticatedLayout,
  },
  mounted() {
    Echo.channel("post").listen("PostCreated", (e) => {
      if (!("Notification" in window)) {
        alert("Web Notification is not supported");
        return;
      }

      Notification.requestPermission((permission) => {
        let notification = new Notification("New post alert!", {
          body: e.post, // content for the alert
        });

        // link to page on clicking the notification
        notification.onclick = () => {
          window.open(window.location.href);
        };
      });
    });
  },
};
</script>
