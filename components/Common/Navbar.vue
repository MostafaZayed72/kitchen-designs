<template>
  <nav ref="navbar" class="navbar navbar-expand-lg">
    <div class="container">
      <!-- Logo -->
      <a href="/" class="logo">
        <img ref="lr" src="/img/logo-light.png" alt="logo"
          v-if="showLogo !== false && !(showDarkLogo && theme === 'light')" />
        <img src="/img/logo-dark.png" alt="logo" v-if="showDarkLogo && theme === 'light'">
      </a>

      <button @click="handleMobileDropdown" class="navbar-toggler" type="button" data-toggle="collapse"
        data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
        aria-label="Toggle navigation">
        <span class="icon-bar">
          <i class="fas fa-bars"></i>
        </span>
      </button>

      <!-- navbar links -->
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav ml-auto">
          <li class="nav-item dropdown" @click="handleDropdown">
            <span class="nav-link dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
              aria-expanded="false">
              Home
            </span>
            <div class="dropdown-menu">
              <a class="dropdown-item" href="/home1">Home Default</a>
              <a class="dropdown-item" href="/home2">Interior Agency</a>
              <a class="dropdown-item" href="/home3">Creative Arch</a>
              <a class="dropdown-item" href="/home4">Architecture Studio</a>
              <a class="dropdown-item" href="/home1-light">Home Light</a>
              <a class="dropdown-item" href="/home2-light">Interior Light</a>
              <a class="dropdown-item" href="/home3-light">Architecture Light</a>
            </div>
          </li>
          <li class="nav-item">
            <NuxtLink class="nav-link" to="/about">About</NuxtLink>
          </li>
          <li class="nav-item dropdown" @click="handleDropdown">
            <span class="nav-link dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
              aria-expanded="false">
              Portfolio
            </span>
            <div class="dropdown-menu">
              <a class="dropdown-item" href="/works1">Two Column</a>
              <a class="dropdown-item" href="/works2">Three Column</a>
              <a class="dropdown-item" href="/works3">Four Column</a>
              <a class="dropdown-item" href="/project-details">Single Project</a>
            </div>
          </li>
          <li class="nav-item dropdown" @click="handleDropdown">
            <span class="nav-link dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true"
              aria-expanded="false">
              Blog
            </span>
            <div class="dropdown-menu">
              <a class="dropdown-item" href="/blog">Blogs</a>
              <a class="dropdown-item" href="/blog-details">Post Details</a>
            </div>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="/contact">Contact</a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script setup>
//= Scripts
import getSiblings from "@/common/getSiblings";

const { lr, theme, nr, showLogo } = defineProps(["lr", "theme", "nr", "showLogo"]);

const showDarkLogo = ref(false);

function handleDropdown(e) {
  getSiblings(e.target.parentElement).filter((item) => item.classList.contains("show")).map((item) => {
    item.classList.remove("show");
    if (item.childNodes[0]) {
      item.childNodes[0].setAttribute("aria-expanded", false);
    }
    if (item.childNodes[1]) {
      item.childNodes[1].classList.remove("show");
    }
  });

  e.target.setAttribute("aria-expanded", true);

  if (e.target.parentElement) {
    e.target.parentElement.classList.toggle("show");
    let dropdownMenu = e.target.parentElement.childNodes[1];
    if (dropdownMenu) {
      dropdownMenu.classList.toggle("show");
    }
  }
}

function handleMobileDropdown(e) {
  document.getElementById("navbarSupportedContent").classList.toggle("show-with-trans");
}

const navbar = ref();

function handleScroll() {
  if (window.scrollY > 300) {
    navbar.value.classList.add("nav-scroll");
    showDarkLogo.value = true;
  }
  else {
    showDarkLogo.value = false;
    navbar.value.classList.remove("nav-scroll");
  }
}

onMounted(() => {
  handleScroll();
  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
})
</script>