<template>
  <nav class="h-full bg-[#000] w-64 px-12 flex flex-col items-center p-10">
    <img src="/globalImages/logo.svg" alt="logo" class="w-16" />
    <div
      class="w-full flex flex-col mt-10 text-md text-white gap-2 [&>a]:flex [&>a]:gap-2 [&>a]:items-center [&>a]:cursor-pointer [&>a]:w-full [&>a>img]:w-4 [&>a>img]:h-4 [&>a>img]:object-contain [&>a>img]:inline-block [&>a>img]:inline-block [&>a>img]"
    >
      <a href="#/"><img src="/globalImages/nav/home.svg" alt="home" />Home</a>
      <a href="#/albums"><img src="/globalImages/nav/albums.svg" alt="albums" />Albums</a>
      <a href="#/artists"><img src="/globalImages/nav/artists.svg" alt="artists" />Artists</a>
      <a href="#/favorites"
        ><img src="/globalImages/nav/favorites.svg" alt="favorites" /> Your favorites</a
      >
    </div>
  </nav>
</template>
