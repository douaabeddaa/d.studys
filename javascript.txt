const API_KEY = AIzaSyAdzen_IwwCIbiGABZUAPzHnNBdVCsnqs4 ; // PASTE YOUR KEY INSIDE THE QUOTES

// Language Switcher
function setLang(lang) {
    const title = document.getElementById('title');
    const desc = document.getElementById('desc');
    const input = document.getElementById('ai-input');

    if (lang === 'ar') {
        title.innerText = "D.studys برو";
        desc.innerText = "بسط أي مادة دراسية. انسخ درسك أو نص الفيديو أدناه.";
        input.placeholder = "انسخ محتوى الدرس هنا...";
        document.body.style.direction = "rtl";
    } else if (lang === 'fr') {
        title.innerText = "D.studys Pro AI";
        desc.innerText = "Analysez n'importe quel sujet. Collez votre leçon ou une transcription YouTube.";
        input.placeholder = "Collez le contenu ici...";
        document.body.style.direction = "ltr";
    } else {
        title.innerText = "D.studys Pro AI";
        desc.innerText = "Analyze any school subject. Paste a lesson or a YouTube transcript below.";
        input.placeholder = "Paste your content here...";
        document.body.style.direction = "ltr";
    }
}

// AI Analysis Logic
async function processLesson(mode) {
    const input = document.getElementById('ai-input').value;
    const responseBox = document.getElementById('ai-response');
    const status = document.getElementById('ai-status');

    if (!input) {
        alert("Please paste your content first! ✨");
        return;
    }

    status.innerText = "D.studys is analyzing your content deeply... 🧠";
    responseBox.innerText = "";

    const baseInstruction = "You are a professional teacher. Analyze the content first, then provide a response that is soft and easy for a student. ";
    
    let prompt = "";
    if (mode === 'simplify') {
        prompt = ${baseInstruction} Simplify this lesson into calm, simple steps: ${input};
    } else {
        prompt = ${baseInstruction} Based on this content, create a 3-question quiz with options A, B, and C. Content: ${input};
    }

    try {
        const response = await fetch(https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}, {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify({
                contents: [{ parts: [{ text: prompt }] }]
            })
        });

        const data = await response.json();
        const result = data.candidates[0].content.parts[0].text;
        
        status.innerText = "Analysis Complete! ✨";
        responseBox.innerText = result;
    } catch (e) {
        status.innerText = "The AI is resting. Try again in a second! 🌸";
        console.error(e);
    }

}
