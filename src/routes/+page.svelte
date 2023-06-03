<script lang='ts'>
  const numbers = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", "."];
  const operations = ["/", "x", "-", "+", "="];
  let selectedOperation='';
  let display='0';

  let firstNumber='';
  //we know to assign to the second number when selectedOperation is assigned
  let secondNumber='';

  let isDisplayingResults = false;

  const handleDisplay=(number:string) => {

    if(isDisplayingResults) {
      handleClear()
    }
    if (number ==='.' && display.includes('.') ){
      return;
    }
    if (number ==='0' && display==='0') {
      return;
    }
    if(!selectedOperation){

      if (display === '0' && number ==='.') {
        firstNumber = '0.';
        return display = firstNumber;
      }
      if (display === '0' ) {
        firstNumber = number;
        return display = firstNumber;
      }
      firstNumber = `${firstNumber}${number}`;
      return display = firstNumber;
    } else {

      if (display === '0' && number ==='.') {
        secondNumber = '0.';
        return display = secondNumber;
      }
      if (display === '0' ) {
        secondNumber = number;
        return display = secondNumber;
      }
      secondNumber = `${secondNumber}${number}`;
      return display = secondNumber;
    }
      //it is eqaul a current display and a number to append it
    display = `${display}${number}`
    
  }
  const handleOperationClick = (operation:string) => {
    //prevent from choosing ex + if first number is not added
    if (firstNumber==='') {
      return;
    }
    if (operation==='=') {
      if (!secondNumber) return;

      const firstNum = parseInt(firstNumber);
      const secondNum = parseInt(secondNumber);
      let results='';

      switch(selectedOperation) {
        case '+':
          results = (firstNum + secondNum).toFixed(2);
          break;
        case '-':
           results = (firstNum - secondNum).toFixed(2);
           break;
        case '/':
          results = (firstNum / secondNum).toFixed(2);
          break;
        case 'x':
          results = (firstNum * secondNum).toFixed(2);
          break;
      }

      console.log('results', results, 'firstNum', firstNum, 'second', secondNum);

      display = results;
      isDisplayingResults =true;
    }
    selectedOperation = operation;
  };

  const handleClear = () => {
    display='0';
    selectedOperation='';
    firstNumber='';
    secondNumber='';
    isDisplayingResults = false;
  }
</script>


<main>
  <h2>It is my first app in Svelte!</h2>
  <p>Let's calculate  :)</p>
  <div class="calculator">
    <div class="results"><h5>{display}</h5></div>
    <div class="digits">
      <div class="numbers">
        <button on:click={()=> handleClear()} class='btn btn-xlg'>C</button>
        <!-- (number in () is treated as a unique identifier as we dont modify this array) -->
        {#each numbers as number(number)}
          <button on:click={()=>handleDisplay(number)} class={`btn ${number === '0' ? 'btn-lg' : null}`}>{number}</button>
        {/each}
      </div>
      <div class="operations">
        {#each operations as operation(operation)}
          <button on:click={()=>handleOperationClick(operation)} class={`btn ${operation === selectedOperation ? "btn-silver": 'btn-orange'}`}>{operation}</button>
        {/each}
      </div>
    </div>
  </div>
</main>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  h2 {
    color: orange;
  }

  p {
     color: rgb(90, 90, 90);
     font-weight: 700;
  }

  main {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 3rem;
  }
  .calculator {
    width: 17rem;
    background-color: pink;
    padding: 2rem;
    border-radius: 1rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .digits {
    display: flex;
  }
  .operations {
    display: flex;
    flex-direction: column;
  }
  .numbers {
    display: flex;
    flex-wrap: wrap;
    width: 10rem;
  }
  .btn {
    width: 2.5rem;
    height: 2.5rem;
    border-radius: 10rem;
    background-color: rgb(114, 113, 113);
    font-size: 1.2rem;
    font-weight: bold;
    color: white;
    margin: .25rem;
    border: none;
  }
  .btn-lg {
    width: 5.5rem;
  }
  .btn-orange {
    background-color: orange;
  }
  .btn-silver {
    background-color: silver;
  }
  .btn-xlg {
    width: 8.5rem;
  }
  .results {
    width: 100%;
    border: 2px solid silver;
    border-radius: .5rem;
    padding: 2rem;
    margin: 2rem;
    height: 6rem;
    color: white;
    font-size: 3.5rem;
    font-weight: 600;
    display: flex;
    flex-direction: row-reverse;
    align-items: center;
    margin-right: 1rem;
    overflow: hidden;
  }
</style>