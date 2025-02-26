

<div style="text-align: center; padding: 20px; border: 1px solid #ddd; max-width: 600px; margin: auto; position: relative;">
  <h2>Spaans Werkwoorden Oefenen</h2>

  <!-- Vraagteken knop in de rechterbovenhoek -->
  <button id="help-button" style="position: absolute; top: 10px; right: 10px; background: none; border: none; font-size: 20px; cursor: pointer;">❓</button>

  <!-- Samenvatting popup voor vervoegingen en klinkerverandering -->
  <div id="help-popup" style="display: none; position: absolute; top: 40px; right: 10px; padding: 15px; background-color: #f1f1f1; border: 1px solid #ddd; max-width: 300px; box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1); font-size: 14px;">
    <div id="help-content">
      <!-- De inhoud wordt dynamisch geladen afhankelijk van het geselecteerde werkwoord -->
    </div>
    <button id="close-popup" style="background: red; color: white; border: none; padding: 5px 10px; cursor: pointer;">Sluit</button>
  </div>

  <!-- Categorieën voor werkwoorden -->
  <p><strong>Kies een categorie werkwoorden:</strong></p>
  <select id="category-selection" style="padding: 8px;">
    <option value="presente">Presente (regelmatige werkwoorden)</option>
    <option value="irregulares">Irregulares (onregelmatige werkwoorden)</option>
  </select>

  <!-- Werkwoordkeuze sectie -->
  <p><strong>Kies een werkwoord om te oefenen:</strong></p>
  <select id="verb-selection" style="padding: 8px;">
    <!-- Dit zal dynamisch worden bijgewerkt op basis van de categorie -->
  </select>

  <p><strong>Oefen de vervoegingen van het gekozen werkwoord!</strong></p>

  <p>Vervoeg het werkwoord <strong id="verb"></strong> voor <strong id="pronoun"></strong>:</p>
  <input type="hidden" id="correct-answer">
  <input type="text" id="user-answer" placeholder="Typ hier je antwoord" style="padding: 8px; width: 80%;"><br><br>

  <!-- Knoppen voor het toevoegen van accenten -->
  <div style="margin-top: 10px;">
    <button onclick="addAccent('é')" style="padding: 5px 10px; margin: 5px;">é</button>
    <button onclick="addAccent('í')" style="padding: 5px 10px; margin: 5px;">í</button>
    <button onclick="addAccent('á')" style="padding: 5px 10px; margin: 5px;">á</button>
  </div><br>

  <button id="check-answer" style="background: blue; color: white; padding: 8px 12px; border: none; cursor: pointer;">Controleer</button>
  <p id="feedback" style="margin-top: 10px; font-weight: bold;"></p>
  <button id="new-exercise" style="background: green; color: white; padding: 8px 12px; border: none; cursor: pointer;">Nieuwe oefening</button>
</div>

<script>
  document.addEventListener("DOMContentLoaded", function () {
    const verbs = {
      presente: {
        hablar: ["hablo", "hablas", "habla", "hablamos", "habláis", "hablan"],
        comer: ["como", "comes", "come", "comemos", "coméis", "comen"],
        vivir: ["vivo", "vives", "vive", "vivimos", "vivís", "viven"]
      },
      irregulares: {
        ser: ["soy", "eres", "es", "somos", "sois", "son"],
        estar: ["estoy", "estás", "está", "estamos", "estáis", "están"],
        tener: ["tengo", "tienes", "tiene", "tenemos", "tenéis", "tienen"],
        ir: ["voy", "vas", "va", "vamos", "vais", "van"],
        hacer: ["hago", "haces", "hace", "hacemos", "hacéis", "hacen"],
        decir: ["digo", "dices", "dice", "decimos", "decís", "dicen"],
        poder: ["puedo", "puedes", "puede", "podemos", "podéis", "pueden"],
        venir: ["vengo", "vienes", "viene", "venimos", "venís", "vienen"],
        saber: ["sé", "sabes", "sabe", "sabemos", "sabéis", "saben"],
        querer: ["quiero", "quieres", "quiere", "queremos", "queréis", "quieren"],
        ver: ["veo", "ves", "ve", "vemos", "veis", "ven"],
        dar: ["doy", "das", "da", "damos", "dais", "dan"],
        caber: ["quepo", "cabes", "cabe", "cabemos", "cabéis", "caben"],
        producir: ["produzco", "produces", "produce", "producimos", "producís", "producen"],
        traer: ["traigo", "traes", "trae", "traemos", "traéis", "traen"],
        huir: ["huyo", "huyes", "huye", "huimos", "huís", "huyen"],
        oír: ["oigo", "oyes", "oye", "oímos", "oís", "oyen"],
        conducir: ["conduzco", "conduces", "conduce", "conducimos", "conducís", "conducen"],
        seguir: ["sigo", "sigues", "sigue", "seguimos", "seguís", "siguen"],
        dormir: ["duermo", "duermes", "duerme", "dormimos", "dormís", "duermen"]
      }
    };

    const pronouns = ["yo", "tú", "él/ella/usted", "nosotros/as", "vosotros/as", "ellos/ellas/ustedes"];

    // Update werkwoorden op basis van de gekozen categorie
    function updateVerbSelection(category) {
      const verbSelection = document.getElementById("verb-selection");
      verbSelection.innerHTML = ""; // Verwijder eerdere keuzes
      const selectedVerbs = verbs[category];

      // Voeg de werkwoorden toe aan de dropdown
      for (const verb in selectedVerbs) {
        const option = document.createElement("option");
        option.value = verb;
        option.textContent = verb;
        verbSelection.appendChild(option);
      }

      // Zet de eerste keuze in de dropdown
      if (verbSelection.options.length > 0) {
        verbSelection.selectedIndex = 0;
      }

      // Werk de vraagteken popup bij met relevante uitleg
      updateHelpContent();
    }

    // Update de uitleg voor de vraagteken knop afhankelijk van het werkwoord
    function updateHelpContent() {
      const selectedVerb = document.getElementById("verb-selection").value;
      const category = document.getElementById("category-selection").value;

      let helpContent = "";
      
      if (category === "presente") {
        // Regelmatige werkwoorden
        helpContent = `
          <p><strong>Samenvatting van vervoegingen voor ${selectedVerb}:</strong></p>
          <p>yo ${verbs.presente[selectedVerb][0]}</p>
          <p>tú ${verbs.presente[selectedVerb][1]}</p>
          <p>él/ella/usted ${verbs.presente[selectedVerb][2]}</p>
          <p>nosotros/as ${verbs.presente[selectedVerb][3]}</p>
          <p>vosotros/as ${verbs.presente[selectedVerb][4]}</p>
          <p>ellos/ellas/ustedes ${verbs.presente[selectedVerb][5]}</p>
        `;
      } else {
        // Onregelmatige werkwoorden
        helpContent = `
          <p><strong>Samenvatting van vervoegingen voor ${selectedVerb} (onregelmatig):</strong></p>
          <p>yo ${verbs.irregulares[selectedVerb][0]}</p>
          <p>tú ${verbs.irregulares[selectedVerb][1]}</p>
          <p>él/ella/usted ${verbs.irregulares[selectedVerb][2]}</p>
          <p>nosotros/as ${verbs.irregulares[selectedVerb][3]}</p>
          <p>vosotros/as ${verbs.irregulares[selectedVerb][4]}</p>
          <p>ellos/ellas/ustedes ${verbs.irregulares[selectedVerb][5]}</p>
          <hr>
          <p><strong>Klinkerverandering:</strong> <em>Deze werkwoorden volgen een klinkerverandering in de stam:</em></p>
          <ul>
            <li><strong>e → ie</strong>: Pensar (pienso), Entender (entiendo)</li>
            <li><strong>o → ue</strong>: Poder (puedo), Dormir (duermo)</li>
            <li><strong>e → i</strong>: Pedir (pido), Servir (sirvo)</li>
          </ul>
        `;
      }

      document.getElementById("help-content").innerHTML = helpContent;
    }

    // Haal willekeurig werkwoord en pronoun
    function getRandomVerb() {
      const category = document.getElementById("category-selection").value;
      const selectedVerb = document.getElementById("verb-selection").value;
      const verbList = verbs[category];
      const pronounIndex = Math.floor(Math.random() * pronouns.length);

      return {
        verb: selectedVerb,
        pronoun: pronouns[pronounIndex],
        correctAnswer: verbList[selectedVerb][pronounIndex]
      };
    }

    // Zet een nieuwe oefening op
    function setupExercise() {
      const { verb, pronoun, correctAnswer } = getRandomVerb();
      document.getElementById("verb").textContent = verb;
      document.getElementById("pronoun").textContent = pronoun;
      document.getElementById("correct-answer").value = correctAnswer;
      document.getElementById("user-answer").value = "";
      document.getElementById("feedback").textContent = "";
    }

    // Controleer het antwoord van de gebruiker
    document.getElementById("check-answer").addEventListener("click", function () {
      const userAnswer = document.getElementById("user-answer").value.toLowerCase();
      const correctAnswer = document.getElementById("correct-answer").value;
      const feedback = document.getElementById("feedback");

      if (userAnswer === correctAnswer) {
        feedback.textContent = "✅ Correct! Goed gedaan!";
      } else {
        feedback.textContent = `❌ Fout! Het juiste antwoord is: ${correctAnswer}`;
      }
    });

    // Maak nieuwe oefening
    document.getElementById("new-exercise").addEventListener("click", setupExercise);

    // Update de werkwoorden als de categorie verandert
    document.getElementById("category-selection").addEventListener("change", function () {
      updateVerbSelection(this.value);
      setupExercise(); // Update de oefening nadat de categorie verandert
    });

    // Update de werkwoorden als het gekozen werkwoord verandert
    document.getElementById("verb-selection").addEventListener("change", function () {
      updateHelpContent();  // Update de uitleg
      setupExercise();  // Update de oefening
    });

    // Functie om accent toe te voegen
    window.addAccent = function (accent) {
      const currentText = document.getElementById("user-answer").value;
      document.getElementById("user-answer").value = currentText + accent;
    };

    // Vraagtekenknop en popup logica
    document.getElementById("help-button").addEventListener("click", function () {
      const popup = document.getElementById("help-popup");
      popup.style.display = popup.style.display === "block" ? "none" : "block";
    });

    // Sluit de popup
    document.getElementById("close-popup").addEventListener("click", function () {
      document.getElementById("help-popup").style.display = "none";
    });

    // Initialisatie
    updateVerbSelection("presente");  // Start met de "presente" categorie
    setupExercise();  // Start de oefening
  });
</script>


