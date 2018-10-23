<template>
  <div>
    <header>
      <div class="header">
        <a href="https://roomclip.jp">
          <img src="https://roomclip.jp/assets/img/1703/logo/logo_roomclip.png">
        </a>
      </div>
    </header>
    <nuxt class="contents" />
    <footer>
      (c) 2018 RoomClip Inc.
    </footer>
  </div>
</template>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

header {
  box-shadow: 0 0.1rem 0 0 rgba(0, 0, 0, 0.1);
}

.header {
  max-width: 400px;
  margin: 0 auto;
}
.header img {
  margin: 1rem 0 .5rem 1.6rem;
}

.contents {
  max-width: 400px;
  margin: 0 auto;
}

footer {
  font-size: 1.2rem;
  color: #443311;
  margin: 2.4rem 0;
  text-align: center;
}
</style>
