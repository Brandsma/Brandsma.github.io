<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Open+Sans:wght@400;600&family=Titillium+Web:wght@400;600;700&display=swap");
:root {
  --primary-color: #ff9c33;
}

body {
  margin: 0;
  font-family: "Open Sans", sans-serif;
  background-color: rgba(0, 0, 0, 0.8);
  height: 100vh;
  display: flex;
  flex-direction: column;
  color: white;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  margin-top: 0;
  font-family: "Titillium Web", sans-serif;
}

p {
  margin-top: 0;
}

* {
  box-sizing: border-box;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.center {
  text-align: center;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

.home,
#app {
  height: 100%;
}

/* buttons */
button {
  font-family: "Titillium Web", sans-serif;
  border: none;
  outline: none;
  padding: 0.5rem 2rem;
  font-weight: 800;
  color: white;
  background-color: var(--primary-color);
  cursor: pointer;
  border-radius: 100px;
  font-size: 1.2rem;
}

/* risk interface */
.risk-interface {
  display: flex;
  flex-direction: column;
  height: 100%;
  padding: 1rem;
}

.risk-interface > header {
  background-color: rgba(0, 0, 0, 0.4);
  color: white;
  padding: 2rem;
  width: 100%;
  border-radius: 1.5rem;
  margin-bottom: 1rem;
}

.risk-container {
  display: flex;
  flex: 1;
  position: relative;
}

.risk-container > * {
  flex: 1;
  display: flex;
  justify-content: center;
  overflow: hidden;
}

.risk-container > *:not(:nth-last-child(1)) {
  margin-right: 1rem;
}

.map-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
}

.map-container > iframe {
  flex: 1;
  width: 100%;
  overflow: hidden;
  border-radius: 1.5rem;
  margin-top: 1rem;
}

/* card */
.card {
  padding: 1rem;
  background-color: #242424;
  box-shadow: 1px 1px 1rem rgba(0, 0, 0, 0.1);
  border-radius: 1.5rem;
  width: 100%;
}

.card .risk:not(:nth-last-child(1)) {
  margin-bottom: 1rem;
}

.var-bar {
  display: flex;
}

.var-bar > *:not(:nth-last-child(1)) {
  margin-right: 0.5rem;
}

/* labels */

.label {
  padding: 0.3rem 0.5rem;
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 100px;
  font-size: 0.8rem;
  color: white;
  font-weight: 800;
}

.risk {
  --risk-color: rgba(0, 0, 0, 0.1);
  border-left: var(--risk-color) solid 0.5rem;
  padding: 1rem;
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 0.5rem;
}

.risk > header {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  margin-bottom: 1rem;
  justify-content: space-between;
}

.risk > header > *:not(:nth-last-child(1)) {
  margin-right: 1rem;
}

.risk > p {
  margin: 0;
}

.risk h3 {
  margin-bottom: 0;
}

.risk-bar {
  border-radius: 0.5rem;
  background-color: var(--risk-color);
  padding: 1rem;
  padding-left: 1.5rem;
  color: white;
  margin-bottom: 1rem;
}

.risk-bar > p {
  margin: 0;
}

.risk-bar > h3 {
  margin-bottom: 1rem;
}

.risk-0 {
  --risk-color: #86b740;
}
.risk-1 {
  --risk-color: #ffc71e;
}
.risk-2 {
  --risk-color: #ff9c33;
}
.risk-3 {
  --risk-color: #ec4646;
}
</style>
