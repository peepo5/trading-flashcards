<script>
  import {Route} from 'tinro'; 
  import Home from './Home.svelte';
  import Cardpage from './components/Cardpage.svelte';

  import ICTjson from './cards/ICT.json';
  import Generaljson from './cards/General.json';
</script>

<Route path="/"><Home /></Route>

<Route path="/flashcards/ict">
  <Cardpage
  card_data={ICTjson}
  />
</Route>
<Route path="/flashcards/general">
  <Cardpage
  card_data={Generaljson}
  />
</Route>

<Route fallback> 
  404 - Lost but not found ;)
</Route>	