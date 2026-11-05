<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import { groupByCause, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  let {
    parserIssue = '',
    hintIssue = '',
  }: {
    parserIssue?: string;
    hintIssue?: string;
  } = $props();

  const merged = $derived(
    uniqueById(reports as Report[]).filter((report) => report.mergedInto === 4471),
  );
  const groups = $derived(groupByCause(merged, tenantOf));
  const parser = $derived(groups.find((group) => group.cause === 'parser'));
  const hint = $derived(groups.find((group) => group.cause === 'hint'));
</script>

<p class="sum">
  The export has <strong>{reports.length}</strong> rows and <strong>{merged.length}</strong> unique
  reports. Tenant and form data separate them into <strong>{groups.length}</strong> causes.
</p>

<table>
  <thead>
    <tr><th>problem</th><th>reports</th><th>tenants</th><th>seen</th><th>ticket</th></tr>
  </thead>
  <tbody>
    {#if parser}
      <tr>
        <td>shared parser treats every date as month-first</td>
        <td>{parser.reports.length}</td>
        <td>{parser.tenants}</td>
        <td>{parser.firstSeen} → {parser.lastSeen}</td>
        <td>{#if parserIssue}<a href={parserIssue}>GitHub issue</a>{:else}pending{/if}</td>
      </tr>
    {/if}
    {#if hint}
      <tr>
        <td>booking-v3 shows a month-first hint to day-first tenants</td>
        <td>{hint.reports.length}</td>
        <td>{hint.tenants}</td>
        <td>{hint.firstSeen} → {hint.lastSeen}</td>
        <td>{#if hintIssue}<a href={hintIssue}>GitHub issue</a>{:else}pending{/if}</td>
      </tr>
    {/if}
  </tbody>
</table>

<style>
  .sum { font-size: 1.1rem; }
</style>
