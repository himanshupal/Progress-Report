<script>
  let subjects = [
    `Hindi`,
    `English`,
    `Maths`,
    `Social`,
    `Science`,
    `G. K.`,
    `Computer`,
    `Sanskrit`,
    `Urdu`
  ];

  let exams = [
    { name: `UT1`, mm: 10 },
    { name: `UT2`, mm: 10 },
    { name: `UT3`, mm: 10 },
    { name: `Half Yearly`, mm: 100 },
    { name: `UT4`, mm: 10 },
    { name: `UT5`, mm: 10 },
    { name: `Yearly`, mm: 100 }
  ];

  let errors = [];
  let totalByExams = [];
  let totalPerSubject = 0;
  let newSubject = ``;
  let newExam = {
    name: "",
    mm: ""
  };
  $: totalMarks = subjects.length * totalPerSubject;

  $: for (let index = 0; index < exams.length; index++) {
    totalByExams = [
      ...totalByExams,
      {
        name: "total" + exams[index].name,
        mm: subjects.length * exams[index].mm
      }
    ];
  }

  $: for (let index = 0; index < exams.length; index++) {
    totalPerSubject += exams[index].mm;
  }

  const addSubject = () => {
    if (newSubject == "") {
      errors = [...errors, "Subject Name can't be blank !"];
    } else {
      errors = [];
      totalByExams = [];
      subjects = [...subjects, newSubject];
      newSubject = ``;
    }
  };

  const addExam = () => {
    if ((newExam.name == "") | (newExam.mm == "")) {
      errors = [...errors, "Both Exam number & Maximum marks are required !"];
    } else {
      errors = [];
      totalByExams = [];
      totalPerSubject = 0;
      exams = [...exams, newExam];
      newExam = { name: "", mm: "" };
    }
  };
</script>

<style>
  .scrollable {
    overflow: auto;
  }
</style>

<div class="scrollable">
  <table>

    <thead>
      <th>
        <em>Subjects</em>
      </th>
      {#each exams as exam}
        <th>{exam.name}</th>
      {/each}
      <th>
        <em>Total</em>
      </th>
    </thead>

    {#each subjects as subject}
      <tbody>
        <tr>
          <th>{subject}</th>
          {#each exams as exam}
            <td>
              <input
                type="number"
                id="{exam.name}{subject}"
                min="0"
                max={exam.mm} />
            </td>
          {/each}
          <td>
            <input
              type="number"
              id="total{subject}"
              min="0"
              max={totalPerSubject} />
          </td>
        </tr>
      </tbody>
    {/each}

    <tfoot>
      <th>
        <em>Total</em>
      </th>
      {#each totalByExams as tExam}
        <td>
          <input type="number" id={tExam.name} min="0" max={tExam.mm} />
        </td>
      {/each}
      <td>
        <input type="number" min="0" max={totalMarks} />
      </td>
    </tfoot>

  </table>
</div>

{#each errors as error}
  <h3>{error}</h3>
{/each}

<form on:submit|preventDefault={addSubject}>
  <input type="text" bind:value={newSubject} />
  <button>Add Subject</button>
</form>

<form on:submit|preventDefault={addExam}>
  Enter Exam Name:
  <input type="text" bind:value={newExam.name} />
  Enter maximum marks:
  <input type="number" bind:value={newExam.mm} />
  <button>Add Exam</button>
</form>

<svelte:head>
  <title>Landing Page</title>
</svelte:head>
