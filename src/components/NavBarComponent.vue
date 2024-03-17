<template>
  <nav
    class="h-full bg-[#000] max-[1050px]:w-auto w-64 px-12 max-[1050px]:p-1 flex flex-col items-center p-10"
  >
    <img src="/globalImages/logo.svg" alt="logo" class="w-16" />
    <div
      class="w-full flex flex-col max-[1050px]:[&>a]:justify-center mt-10 text-md text-white max-[1050px]:[&>a>img]:h-8 max-[1050px]:[&>a>img]:w-8 max-[1050px]:[&>a>span]:hidden gap-2 [&>a]:flex [&>a]:gap-2 [&>a]:items-center [&>a]:cursor-pointer [&>a]:w-full [&>a>img]:w-4 [&>a>img]:h-4 [&>a>img]:object-contain [&>a>img]:inline-block [&>a>img]:inline-block [&>a>img]"
    >
      <a href="#/"><img src="/globalImages/nav/home.svg" alt="home" /><span class="">Home</span></a>
      <a href="#/albums"
        ><img src="/globalImages/nav/albums.svg" alt="albums" /><span>Albums</span>
      </a>
      <a href="#/artists"
        ><img src="/globalImages/nav/artists.svg" alt="artists" /><span>Artists</span></a
      >
      <a href="#/favorites"
        ><img src="/globalImages/nav/favorites.svg" alt="favorites" /><span>
          Your favorites</span
        ></a
      >
    </div>
  </nav>
</template>
