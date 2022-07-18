<template>
  <div>
    <header>
      <h2 id="version">Version 1.0.0</h2>
    </header>
    <main>
      <section>
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
    </main>
  </div>
  <router-link to="/">BACK</router-link>
</template>

<script>
export default {
  name: "AboutPage",
};
</script>

<style scoped>
header {
  text-align: center;
}

#version {
  font-style: italic;
}

main {
  padding: 1rem;
  max-width: 95%;
  margin-inline: auto;
}

.section-title {
  text-transform: uppercase;
  font-size: 2rem;
}
.external-link{
    color: #0070f3;
    font-style: italic;
}

section {
  display: flex;
  flex-direction: column;
  gap: 1em;
}

p {
  max-width: 70ch;
}
</style>
