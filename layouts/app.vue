<template>
  <div>
    <b-navbar
      toggleable
      type="dark"
      variant="dark"
      class="mb-3">
      <b-navbar-toggle target="nav_text_collapse"/>
      <b-navbar-brand>Guestbook</b-navbar-brand>
      <b-collapse
        id="nav_text_collapse"
        is-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item
            to="/"
            exact>
            Home
          </b-nav-item>
          <b-nav-item
            to="/signatures"
            exact>
            Signatures
          </b-nav-item>
          <b-nav-item
            to="/signatures/create"
            exact>
            Sign
          </b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <b-container>
      <nuxt/>
    </b-container>
  </div>
</template>
