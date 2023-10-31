<div>
    <h1>Diagrama de Gantt </h1>
    <p> diagrama de gantt creado con svelte y typeScript</p>
</div>
<br>
<div>
    <table>
        <thead>
            <tr>
                <th>Nombre</th>
                {#each actual as actual }
                    <th>{actual}</th>
                {/each}
                <th>Dias totales</th>
            </tr>
        </thead>
        <tbody>
            {#each gant as gant}
                <tr>
                    <th id="name">{gant[0]}</th>
                    {#each antes(gant[2]) as {}}
                        <td></td>
                    {/each}
                    <td colspan={colspan(gant[2], gant[3])}><li style="background-color: {color(gant[2],gant[3])}; width:{anch(gant[2],gant[3])}*10%;">{gant[1]}</li></td>
                    {#each despues(gant[3]) as {}}
                        <td></td>
                    {/each}
                    <th>{dias(gant[2],gant[3])}</th>
                </tr>
            {/each}
        </tbody>
    </table>
</div>

<script lang="ts">
    $: gant=([  ['Raul', 'Action', '2023-02-10', '2023-02-20'],
				['Sofia', 'Action', '2023-02-20', '2023-03-10'],
				['Rafa', 'Action', '2023-03-14', '2023-06-17'],
				['Laura', 'Action', '2023-05-18', '2023-06-22'],
				['Karla', 'Action', '2023-06-19', '2023-07-22'],
				['Martin', 'Action', '2023-07-12', '2023-09-20']
                //nombre, que hace, fecha inicial,  fecha final
            ]);
    const actual: string[] = ["Enero", "Febrero", "Marzo", "Abril", "Mayo", 
                              "Junio", "Julio", "Agosto", "Septiembre", 
                              "Octubre", "Noviembre", "Diciembre"
                             ];
    

    function antes(min: string | number | Date){
        let diaMin: Date = new Date(min);
        let ddMin: number = diaMin.getMonth();       //obtengo el num mes
        let dato: string[] = [];
        for(let j = 0; j < ddMin; j++){
            dato.push('');
        }
        return dato;
    }
    function despues(max: string | number | Date){
        let diaMax: Date = new Date(max);
        let ddMax: number = diaMax.getMonth();       //obtengo el num mes
        let dddMax = (11- ddMax)
        let dato: string[] = [];
        for(let j = 0; j < dddMax; j++){
            dato.push('');
        }
        return dato;
    }
    /*
    funcion para determinar los meses
    */
    function colspan(min: string | number | Date, max: string | number | Date){
        let diaMin = new Date(min);
        let ddMin: number = diaMin.getMonth();       //obtengo el num mes
        let dddMin = ddMin +1;

        let diaMax = new Date(max);
        let ddmax: number = diaMax.getMonth();       //obtengo el num mes
        let dddMax = ddmax + 1;

        let diferencias = (dddMax - dddMin);
        return diferencias + 1;
    }
    /*
    Funcion para los dias transcurridos
    */
    function dias(min: string | number | Date, max: string | number | Date){
        let diaMin:any = new Date(min);
        let diaMax:any = new Date(max);
        let diffTime =  Math.abs(diaMax - diaMin);
        return Math.ceil(diffTime/ (86400000));       //tomaremos milisegundos de un dia
    }
    /*
    Funcion para determinar el color con base a los dias transcurridos
    */
    function color(min: string | number | Date, max: string | number | Date){
        let days: number = dias(min, max);
        let color: string = "#ffffff";
        if(days <= 20){                                     
            return color = "#7FFF00";                                             
        }else if(days > 20 && days <= 50){                  
            return color = "#FF8C00";                                
        }else if(days > 50){                                
            return color = "#B22222";                                
        }
    }

    /*
    Funicion para determinar el ancho del estilo
    */
    function anch(min: string | number | Date, max: string | number | Date){
        let days: number = dias(min, max);
        let dayss: number = days;
        if(days <= 20){                                      
            if(days >= 10){
                return dayss = days/2;
            }                                                  
            if(days <= 5){                                     
                return dayss = (days * 2);                              
            }                                             
            }else if(days > 20 && days <= 50){
                dayss = (days/2);             
                if(dayss >= 10){
                    return dayss = 7
                }
            }else if(days > 50){
                return dayss = (days/10)/1.1                              
            } 
    }
</script>

<style>
    *{
        padding: 0;
        margin: 0;
        box-sizing: border-box;
        text-align: center;
	    margin: 0 auto;
    }
    table{
        border-collapse: collapse;
        border-spacing: 0px 20px;
        border-bottom: 2px solid #7D7C7C;
    }
    table, tr, th{
	    border: 2px solid #7D7C7C;
	    border-collapse: collapse;
	    padding: 8px;
	    font-size: 12px;
	    background-color: #FFF5E0;
	    line-height: 25px;
    }
    li{
	    list-style: none;
    }
    table, tr, td, li{
	    background-color: #FFF5E0;
	    padding: 5px 10px;
	    border-radius: 2rem;
	    margin-right: 10px;
    }
</style>
