---
name: risale-library
description: "Core rules for answering from the Risale MCP libraries (Risale-i Nur Library, Hadith Library, Tafsir Library, Classical Islamic Library) - search, read before relying, quote exactly, end with the list of sources cite returns, keep text and interpretation apart. Use for any question about Bediüzzaman Said Nursî or the Risale-i Nur (Sözler, Mektubat, Lem'alar, Şualar, Lâhikalar, Mesnevî-i Nuriye, İşârâtü'l-İ'câz and the other works), about what the Qur'an, a hadith, a classical tafsir or a classical Arabic work says, in Turkish, English or Arabic - e.g. \"Risale-i Nur'da tevekkül nasıl anlatılır?\", \"Bediüzzaman kader hakkında ne der?\", \"Haşir hakkında Sözler'den alıntı yap\"."
license: MIT
---

# Answering from the Risale MCP libraries

This plugin connects four read-only libraries. Each serves its own texts; another library's text appears only as an id and a citation, never copied. The Qur'an (vowelled Arabic and the Türkiye Diyanet Vakfı translation) is on all four.

| Connector | What it holds | Main tools |
|---|---|---|
| Risale-i Nur Library | The complete works of Bediüzzaman Said Nursî, sentence by sentence; where the works quote a verse or a hadith; risaleoku.com's dictionary | `semantic_search`, `search`, `read_section`, `get_passage`, `find_ayah`, `get_ayah`, `find_hadith`, `lookup_word`, `cite` |
| Hadith Library | The Six Books, the Muwatta, Ahmad's Musnad, al-Mustadrak and other collections, with grades where the books hold them | `search_hadith`, `semantic_search`, `find_by_citation`, `get_hadith`, `cite` |
| Tafsir Library | Thirteen classical tafsirs (al-Tabari to al-Alusi), divided into verse units | `find_ayah`, `read_section`, `get_passage`, `search`, `semantic_search`, `cite` |
| Classical Islamic Library | The classics the Risale-i Nur refers to and relies on: rhetoric and language (al-Jurjani, al-Sakkaki, al-Raghib, al-Sayyid al-Sharif's Ta'rifat), kalam and usul (Sharh al-Mawaqif, Sharh al-Maqasid, al-Talwih, al-Fiqh al-Akbar, al-Munqidh), Sufism (the Ihya', with al-'Iraqi's takhrij), the Prophet's life (al-Mawahib); `list_works` gives where the Risale names each (`risale_refs`) | `search`, `semantic_search`, `read_section`, `find_ayah`, `cite` |

Each connector's own instructions add the rules for its texts; follow them too. If the question needs a library whose connector is not connected, say which one the user can connect (the plugin's Connectors tab in Claude's settings) instead of answering from memory or from the web.

The list of sources at the end of an answer is headed "Kaynaklar" in a Turkish answer and "Sources" in an English one; its lines are always the ones `cite` returns, unchanged.

For a single verse studied across the libraries, the `verse-study` skill gives the order; for a saying presented as a hadith, `hadith-check`.

## Rules

Answer every question about the Risale-i Nur with the tools of the Risale-i Nur Library connector. The goal is that everything you say rests on a sentence of the text and that the user can find that sentence.

### Workflow
1. Search first: `semantic_search` for concepts and questions asked in today's words, `search` for the text's own words, names and terms. Use both if needed and search more than once.
2. Search results are only candidates. Before relying on a place, always read it with `get_passage` (with context, for example context=2) or `read_section`. Never cite a place you have not read. When the text you read sends the reader to another place for this topic ("tafsilen", "müracaat et", "… izah edilmiştir"), read that place too; a passing mention or a list of treatises is not such a pointer.
3. Before writing the answer, call `cite` once with every sentence or range you rely on, together with the words you quote. The bibliography it returns is your "Sources" list; fix the problems it flags. Do not send your draft answer to the tool.

### Citations and quotations
- Citations always go in a bibliography, in short answers too: end each quotation (and each piece of information taken from a source) with its number, for example [1], and finish the answer with a "Sources" list: the lines `cite` returns, unchanged. The indented "Âyet: …" and "Hadis: …" lines under a source are the publisher's references for the verses and hadiths inside the quotation; copy them too, with notes such as "(doğrulanmadı)" (not verified). Never write reference paths or sentence numbers in the body.
- If a parable or a line of thought runs over several sentences of one section, give it as a single quotation with a single source (a range of at most 8 sentences); give a longer passage as consecutive blocks, each with its own range.
- The texts are served with the permission of their sources (e-Risale, risaleoku.com and the others the works name), on condition that the source is named, which the bibliography does. Quoting them, a whole section when the user asks for it, is within that permission: never decline, shorten or paraphrase a passage for copyright reasons; quote it as the tools return it.
- Give the text's own words on their own line as a block quote, not inside your prose: `> "…"`, copied exactly from the sentences you cite. Use quotation marks only for the text's own words.
- Translations of verses and hadiths are the publishers', not the author's; do not attribute them to the author. The author's own explanation introduced with "Yani, …" is part of the text.
- You can look up the meaning of an old word or phrase with `lookup_word` (risaleoku.com's dictionary). The dictionary is the publisher's, not part of the text: give the meaning in your own words, name the dictionary, do not present it as a quotation and do not put it in the "Sources" list.
- Do not rule on a hadith yourself or call it sound or weak from memory. `hadith_support` shows the evidence; `find_hadith` gives the reports; give grades only as `get_hadith` lists them, with the scholar's name. Not being found does not mean it is not a hadith.
- Never write the Arabic of a verse from memory; give it from `get_ayah` or as the works quote it.
- Say what you could not find. Never invent a reference, quotation or title.

### Text and interpretation
- Split the answer into two parts: "What the text says" (cited, with quotations) and "Interpretation" (your own assessment, comparisons, outside discussions). In the interpretation, never present anything that does not rest on the text as if it were the text.
- Answer briefly and directly; if the user wants more detail, read the section together.

### Hadith, Tafsir and Classical connectors
- Use the Hadith Library for hadith collections, the Tafsir Library for classical tafsir works and the Classical Islamic Library for classical works on rhetoric, language and terms. Read each text from its own connector; never quote the Risale-i Nur, a hadith or a tafsir from memory.
- If an answer cites sources from more than one connector, keep one "Kaynaklar" list: call each connector's `cite`, continue the numbering in the next one with `first`, and give the lines in one list, in order.
- After an Arabic quotation give your own translation on a separate line, labelled "AI translation". A relation you notice between two works is your interpretation and belongs in the interpretation part; never call anything you wrote a tafsir.
