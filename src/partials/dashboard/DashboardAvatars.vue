<template>
  <ul class="flex flex-wrap justify-center sm:justify-start mb-8 sm:mb-0 -space-x-3 -ml-px">
    <li>
      <router-link class="block" to="#0">
        <img class="w-9 h-9 rounded-full" src="../../images/user-36-01.jpg" width="36" height="36" alt="User 01" />
      </router-link>
    </li>
    <li>
      <router-link class="block" to="#0">
        <img class="w-9 h-9 rounded-full" src="../../images/user-36-02.jpg" width="36" height="36" alt="User 02" />
      </router-link>
    </li>
    <li>
      <router-link class="block" to="#0">
        <img class="w-9 h-9 rounded-full" src="../../images/user-36-03.jpg" width="36" height="36" alt="User 03" />
      </router-link>
    </li>
    <li>
      <router-link class="block" to="#0">
        <img class="w-9 h-9 rounded-full" src="../../images/user-36-04.jpg" width="36" height="36" alt="User 04" />
      </router-link>
    </li>
    <li>
      <button class="flex justify-center items-center w-9 h-9 rounded-full bg-white border border-gray-200 hover:border-gray-300 text-indigo-500 shadow-sm transition duration-150 ml-2">
        <span class="sr-only">Add new user</span>
        <svg class="w-4 h-4 fill-current" viewBox="0 0 16 16">
          <path d="M15 7H9V1c0-.6-.4-1-1-1S7 .4 7 1v6H1c-.6 0-1 .4-1 1s.4 1 1 1h6v6c0 .6.4 1 1 1s1-.4 1-1V9h6c.6 0 1-.4 1-1s-.4-1-1-1z" />
        </svg>
      </button>
    </li>
  </ul>
</template>

<script>
export default {
  name: 'DashboardAvatars',
}
</script>