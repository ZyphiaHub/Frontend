<script>
    // @ts-nocheck
      var jv = false
      switch
      case 1
      const asz = 15
      var pont = 0
      var aknak = Array.from({length: asz}, _ => '💣')
      .concat (Array.from({length: 100 - asz}, _ => ' '))
      .sort((a, b) => Math.random() - 0.5)
      var t = Array.from({length: 10}, (_, y) => 
        Array.from({length: 10}, (_, x) => aknak[y * 10 + x])
      )
    </script>
    <main>
      <h1>Aknakereső</h1>
      <p>Megtalált aknák:  {pont} <br>
        Még hátra van: {asz - pont}

      </p>
      <table>
        {#each t as row, y}
          <tr>
            {#each row as cell, x}
              <td on:click={() => {
                if (jv) return
                if (cell === '💣') {
                  cell = `💀`
                  jv = true
                } else {
                  let asz = 0;
                  [1,0,-1].forEach(i => [1,0,-1].forEach(j => {
                    if (
                      t[y+i] && t[y+i][x+j] === '💣' ||
                      t[y+i] && t[y+i][x+j] === 'Z'
                    ) asz++ 
                  }))
                  cell = asz
                  if (cell === 0) {
                    
                  }
                }
              }} on:contextmenu|preventDefault = {() => {
                if ('zZ'.includes(cell)) {
                  cell = ' '
                  return
                }
                if (cell === '💣') {
                  cell = "Z"
                  pont++

                } else {
                  cell = "z"
                }
              }}>{cell === '💣' ? ' ' : 
                  'zZ'.includes(cell) ? `📍` : cell  }</td>
            {/each}
          </tr>
        {/each}
      </table>
    </main>
    <style>
      td {
        border: 1px solid black;
        width: 40px;
        height: 40px;
        text-align: center;
        vertical-align: middle;
        font-size: 24px;
        cursor: pointer;
        background-color: rgb(150, 138, 123);
        border-radius: 8px;
        box-shadow: 1px 1px 3px inset black;
      }
      td:hover {
        background-color: lightgray;
      }
    </style>