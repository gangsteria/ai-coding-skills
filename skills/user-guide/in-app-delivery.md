# In-app delivery

- **Source:** the route renders the Markdown files from the guide location at build time,
  so each release ships the guide that describes it.
- **Route:** one guide route, named in the guide language the way the app names its other
  routes. It shows the index; each page sits at a sub-path taken from its file name.
- **Navigation link:** one link labelled in the guide language. It goes in the app's
  persistent navigation or menu when there is one, otherwise beside the account or session
  actions.
- **Access:** a page is visible to whoever can reach the screens it describes, so each
  audience sees its own pages. A page for a goal the user has before signing in, such as
  signing in or accepting an invitation, must be reachable signed out. Ask the user which
  audiences' pages, if any, are open to everyone.

Build the route as its own change, following the project's conventions for routes,
components, tests and review.

Done when: the route shows the index, every page opens from it for the audiences the
access rule gives it, and the navigation link is visible at the viewport in the settings.
