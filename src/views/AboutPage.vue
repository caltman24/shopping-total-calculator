<template>
  <div id="about">
    <header>
      <h1 id="title">Shopping total calculator</h1>
      <h2 id="version">Version 1.0.0</h2>
    </header>
    <main>
      <section class="sales-tax-info">
        <h2 class="section-title">Sales Tax</h2>
        <p>
          Sales and use tax rates change on a monthly basis. All sales tax data
          is sourced from
          <a
            href="https://www.salestaxinstitute.com/resources/rates"
            target="_blank"
            class="external-link"
          >
            Sales Tax Institute</a
          >
          and was last updated as of 7/1/2022
        </p>
        <p>
          Some local jurisdictions do not impose a sales tax. Shopping total
          calculator does not calculate the varying local taxes, and will
          calculate at 0% tax for those states without a sales tax
        </p>
      </section>
      <section class="get-started-info">
        <h2 class="section-title">Getting Started</h2>
        <ul>
          <li>
            <strong>Selecting your State</strong> - Select the state where you
            are shopping from the dropdown menu and Total Shopping Calculator
            will calculate the sales tax for you based off the state rate.
          </li>
          <li>
            <strong>Adding an Item</strong> - Tap the "Add Item" Button then
            enter the name(optional) and price(required) of the item then tap
            "save item"
          </li>
          <li>
            <strong>Removing an Item</strong> - Double tap the item to remove it
          </li>
          <li>
            <strong>Food</strong> - When adding an item, the item is marked as
            food by default. If you want to mark an item as non-food, uncheck
            the "food" checkbox. Food does not get taxed at the state level.
            <ul>
              <li id="food-clr-sample"><strong>Food</strong> - light purple</li>
              <li id="nonfood-clr-sample">
                <strong>Non-Food</strong> - dark purple
              </li>
            </ul>
          </li>
        </ul>
      </section>
    </main>
    <router-link to="/">BACK</router-link>
  </div>
</template>

<script>
export default {
  name: "AboutPage",
};
</script>

<style scoped>
#about {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  padding-bottom: 2em;
}

header {
  text-align: center;
}

#version {
  font-style: italic;
}

main {
  margin-inline: auto;
  display: flex;
  flex-direction: column;
  gap: 2em;
  align-items: center;
}

.section-title {
  text-transform: uppercase;
  font-size: 2rem;
}
.external-link {
  color: #0070f3;
  font-style: italic;
}

section {
  display: flex;
  flex-direction: column;
  gap: 1em;
  width: 100%;
  max-width: 70ch;
  padding: 1em;
}

ul {
  padding: 1em 2em;
}

#food-clr-sample {
  color: var(--clr-food);
}

#nonfood-clr-sample {
  color: var(--clr-nonfood);
}
</style>
