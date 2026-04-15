const { addonBuilder, serveHTTP } = require("stremio-addon-sdk");
const manifest = {
  id: "org.moefawaz.stremio.addon",
  version: "1.0.0",
  name: "Moe's Stremio Addon",
  description: "Custom addon with catalog, streams and subtitles.",
  logo: "https://dl.strem.io/addon-logo.png",
  background: "https://dl.strem.io/addon-background.jpg",
  contactEmail: "admin@example.com",
  behaviorHints: { adult: false, p2p: true, configurable: false, configurationRequired: false },
  types: ["movie", "series"],
  idPrefixes: ["tt"],
  resources: [
    { name: "catalog", types: ["movie", "series"], idPrefixes: ["tt"] },
    { name: "meta", types: ["movie", "series"], idPrefixes: ["tt"] },
    { name: "stream", types: ["movie", "series"], idPrefixes: ["tt"] },
    { name: "subtitles", types: ["movie", "series"], idPrefixes: ["tt"] }
  ],
  catalogs: [
    { type: "movie", id: "top_movies", name: "Top Movies", extra: [{ name: "search", isRequired: false }, { name: "skip", isRequired: false }] },
    { type: "series", id: "top_series", name: "Top Series", extra: [{ name: "search", isRequired: false }, { name: "skip", isRequired: false }] }
  ]
};
const builder = new addonBuilder(manifest);
const MOVIES = [
  { id: "tt0111161", name: "The Shawshank Redemption", poster: "https://m.media-amazon.com/images/M/MV5BNDE3ODcxYzMtY2YzZC00NmNlLWJiNDMtZDViZWM2MzIxZDYwXkEyXkFqcGdeQXVyNjAwNDUxODI@._V1_SX300.jpg", year: 1994 },
  { id: "tt0068646", name: "The Godfather", poster: "https://m.media-amazon.com/images/M/MV5BM2MyNjYxNmUtYTAwNi00MTYxLWJmNWYtYzZlODY3ZTk3OTFlXkEyXkFqcGdeQXVyNzkwMjQ5NzM@._V1_SX300.jpg", year: 1972 },
  { id: "tt0468569", name: "The Dark Knight", poster: "https://m.media-amazon.com/images/M/MV5BMTMxNTMwODM0NF5BMl5BanBnXkFtZTcwODAyMTk2Mw@@._V1_SX300.jpg", year: 2008 },
  { id: "tt0050083", name: "12 Angry Men", poster: "https://m.media-amazon.com/images/M/MV5BMWU4N2FjNzYtNTVkNC00NzQ0LTg0MjAtYTJlMjFhNGUxZDFmXkEyXkFqcGdeQXVyNjc1NTYyMjg@._V1_SX300.jpg", year: 1957 },
  { id: "tt0108052", name: "Schindler's List", poster: "https://m.media-amazon.com/images/M/MV5BNDE4OTEyMzUtNmNkMC00NjhkLTlkZjctZjIxZjA5ZGVlZDM5XkEyXkFqcGdeQXVyNjU0OTQ0OTY@._V1_SX300.jpg", year: 1993 }
];
const SERIES = [
  { id: "tt0903747", name: "Breaking Bad", poster: "https://m.media-amazon.com/images/M/MV5BYmQ4YWMxYjUtNjZmYi00MDdmLWJjOTUtYjc2OGUwZjE0ZGJkXkEyXkFqcGdeQXVyMTMzNDExODE5._V1_SX300.jpg", year: 2008 },
  { id: "tt0944947", name: "Game of Thrones", poster: "https://m.media-amazon.com/images/M/MV5BYTRiNDQwYzAtMzVlZS00NTI5LWJjYjUtMzkwNTUzMWMxZTllXkEyXkFqcGdeQXVyNDIzMzcwNjc@._V1_SX300.jpg", year: 2011 },
  { id: "tt4574334", name: "Stranger Things", poster: "https://m.media-amazon.com/images/M/MV5BN2ZmYjg1YmItNWQ4OC00YWM0LWE0ZDktYThjOTZiZjhhN2Q2XkEyXkFqcGdeQXVyNjgxNTQ3Mjk@._V1_SX300.jpg", year: 2016 },
  { id: "tt1475582", name: "Sherlock", poster: "https://m.media-amazon.com/images/M/MV5BMWY3NTljMjEtYzRiMi00NWM2LTkzNjItZTVmZjE0MTdjMjJhL2ltYWdlXkEyXkFqcGdeQXVyNTQ4NTc5OTU@._V1_SX300.jpg", year: 2010 },
  { id: "tt0306414", name: "The Wire", poster: "https://m.media-amazon.com/images/M/MV5BZmY5ZDMxOWMtOGMzZS00OGY5LTgwZDEtMWU4YjY0NmM0NDZlXkEyXkFqcGdeQXVyNjc3MjQzNTI@._V1_SX300.jpg", year: 2002 }
];
builder.defineCatalogHandler(({ type, id, extra }) => {
  let metas = type === "movie" ? MOVIES.map(m => ({ id: m.id, type: "movie", name: m.name, poster: m.poster, year: m.year })) : SERIES.map(s => ({ id: s.id, type: "series", name: s.name, poster: s.poster, year: s.year }));
  if (extra && extra.search) { const q = extra.search.toLowerCase(); metas = metas.filter(m => m.name.toLowerCase().includes(q)); }
  return Promise.resolve({ metas });
});
builder.defineMetaHandler(({ type, id }) => {
  const item = [...MOVIES, ...SERIES].find(m => m.id === id);
  if (!item) return Promise.resolve({ meta: null });
  return Promise.resolve({ meta: { id: item.id, type, name: item.name, poster: item.poster, year: item.year, description: `${item.name} (${item.year})` } });
});
builder.defineStreamHandler(({ type, id }) => {
  const DEMO_STREAMS = {
    "tt0111161": [{ title: "1080p BluRay", infoHash: "dd8255ecdc7ca55fb0bbf81323d87062db1f6d1c", fileIdx: 1 }],
    "tt0468569": [{ title: "1080p BluRay", infoHash: "209c8226b299b308beaf2b9cd3fb49212dbd13ec", fileIdx: 1 }]
  };
  const streams = DEMO_STREAMS[id] || [{ title: "Demo Stream", url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4" }];
  return Promise.resolve({ streams });
});
builder.defineSubtitlesHandler(({ type, id }) => {
  return Promise.resolve({ subtitles: [{ id: `${id}_en`, url: "https://raw.githubusercontent.com/YePpHa/maia-yt/master/test/subtitles/test.srt", lang: "eng", name: "English (Demo)" }] });
});
const PORT = process.env.PORT || 7000;
serveHTTP(builder.getInterface(), { port: PORT });
console.log(`Addon running at http://localhost:${PORT}/manifest.json`);
