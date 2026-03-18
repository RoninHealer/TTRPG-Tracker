<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>Adventurer's Ledger</title>
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700&family=Crimson+Pro:ital,wght@0,300;0,400;0,600;1,400&display=swap" rel="stylesheet"/>
<style>
:root{
  --gold:#D4AF37;--gold-dim:#8a6e1a;--gold-glow:#D4AF3740;
  --silver:#C0C0C0;--copper:#B87333;
  --green:#2ecc71;--red:#e74c3c;--blue:#3498db;--orange:#f39c12;
  --bg:#0f0d07;--bg2:#161209;--bg3:#1c1608;--bg4:#221c0a;
  --border:#2a2010;--border2:#362e14;
  --text:#d4c9a8;--text-dim:#887a5a;--text-muted:#4a4030;
  --font-d:'Cinzel',serif;--font-b:'Crimson Pro',serif;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{background:var(--bg);color:var(--text);font-family:var(--font-b);font-size:15px;min-height:100vh}
body{background-image:radial-gradient(ellipse at 15% 10%,#1a140040 0,transparent 55%),radial-gradient(ellipse at 85% 90%,#0a0d1840 0,transparent 55%)}

/* HEADER */
.hdr{text-align:center;padding:18px 16px 0;border-bottom:1px solid var(--border);position:relative}
.hdr h1{font-family:var(--font-d);font-size:1.5rem;color:var(--gold);letter-spacing:.1em;text-shadow:0 0 24px #D4AF3750}
.hdr-sub{font-size:.76rem;color:var(--text-muted);font-style:italic;margin-top:3px;padding-bottom:10px}

/* SAVE FLASH */
#save-flash{
  position:fixed;bottom:14px;right:14px;z-index:999;
  background:var(--bg2);border:1px solid #2ecc7150;border-radius:5px;
  padding:7px 13px;font-family:var(--font-d);font-size:.62rem;
  color:var(--green);letter-spacing:.08em;
  opacity:0;transition:opacity .3s;pointer-events:none;
  box-shadow:0 4px 20px #00000060;
}

/* PROFILE BAR */
.prof-bar{display:flex;align-items:center;gap:7px;padding:9px 12px;background:var(--bg2);border-bottom:1px solid var(--border)}
.prof-icon{font-size:1.05rem;flex-shrink:0}
.prof-lbl{font-size:.6rem;color:var(--text-muted);font-family:var(--font-d);letter-spacing:.08em;flex-shrink:0}
.prof-wrap{position:relative;flex:1;min-width:0}
.prof-sel{width:100%;background:var(--bg3);border:1px solid var(--border2);border-radius:5px;padding:6px 26px 6px 10px;font-family:var(--font-d);font-size:.7rem;letter-spacing:.04em;color:var(--gold);cursor:pointer;appearance:none;outline:none;transition:border-color .2s}
.prof-sel:focus{border-color:var(--gold-dim)}
.prof-wrap::after{content:'▾';position:absolute;right:8px;top:50%;transform:translateY(-50%);color:var(--gold-dim);pointer-events:none;font-size:.66rem}
.pbtn{padding:5px 9px;background:transparent;border:1px solid var(--border);border-radius:4px;font-family:var(--font-d);font-size:.56rem;letter-spacing:.04em;color:var(--text-muted);cursor:pointer;transition:all .2s;white-space:nowrap;flex-shrink:0}
.pbtn:hover{border-color:var(--border2);color:var(--text)}
.pbtn.g{border-color:#D4AF3740;color:var(--gold)}.pbtn.g:hover{border-color:var(--gold);background:#D4AF3710}
.pbtn.r{border-color:#e74c3c20;color:#e74c3c50}.pbtn.r:hover{border-color:var(--red);color:var(--red)}

/* TABS */
.tab-bar{display:flex;overflow-x:auto;background:var(--bg2);border-bottom:1px solid var(--border);scrollbar-width:none}
.tab-bar::-webkit-scrollbar{display:none}
.tab-btn{flex-shrink:0;padding:10px 12px;background:none;border:none;border-bottom:2px solid transparent;font-family:var(--font-d);font-size:.62rem;letter-spacing:.07em;color:var(--text-muted);cursor:pointer;white-space:nowrap;transition:all .2s}
.tab-btn:hover{color:var(--text-dim)}
.tab-btn.active{color:var(--gold);border-bottom-color:var(--gold);background:linear-gradient(180deg,transparent,#D4AF3708)}

/* PANELS */
.panel{display:none;padding:16px;animation:fadeIn .22s ease}
.panel.active{display:block}
@keyframes fadeIn{from{opacity:0;transform:translateY(4px)}to{opacity:1;transform:none}}

.sec{font-family:var(--font-d);font-size:.64rem;letter-spacing:.16em;color:var(--text-dim);text-transform:uppercase;margin-bottom:12px;padding-bottom:7px;border-bottom:1px solid var(--border)}

/* CARDS */
.card{background:var(--bg3);border:1px solid var(--border);border-radius:6px;padding:11px 13px;margin-bottom:8px;transition:border-color .2s}
.card:hover{border-color:var(--border2)}
.card-row{display:flex;align-items:flex-start;gap:10px}
.ci{width:34px;height:34px;border-radius:5px;background:var(--bg4);border:1px solid var(--border);display:flex;align-items:center;justify-content:center;font-size:1.2rem;flex-shrink:0}
.cb{flex:1;min-width:0}
.cn{font-family:var(--font-d);font-size:.82rem;color:var(--text);margin-bottom:3px}
.cm{font-size:.73rem;color:var(--text-muted);display:flex;flex-wrap:wrap;gap:6px;align-items:center}
.ca{display:flex;gap:4px;flex-shrink:0}
.cd{font-size:.79rem;color:var(--text-dim);margin-top:6px;font-style:italic;border-top:1px solid var(--border);padding-top:6px;line-height:1.4}

.badge{font-size:.6rem;padding:2px 6px;border-radius:3px;border:1px solid;font-family:var(--font-d);letter-spacing:.04em;white-space:nowrap}
.empty{text-align:center;padding:30px 16px;color:var(--text-muted);font-style:italic;font-size:.84rem}

/* FORMS */
.fbox{background:var(--bg2);border:1px solid var(--border);border-radius:6px;padding:13px;margin-bottom:13px}
.fr{display:flex;flex-wrap:wrap;gap:7px;margin-bottom:7px}
.fr:last-child{margin-bottom:0}
.inp{background:var(--bg3);border:1px solid var(--border);border-radius:4px;padding:7px 10px;font-family:var(--font-b);font-size:.9rem;color:var(--text);outline:none;transition:border-color .2s}
.inp:focus{border-color:var(--gold-dim)}
.inp::placeholder{color:var(--text-muted)}
.inp.f1{flex:1;min-width:100px}.inp.fw{width:100%}.inp.sm{width:76px}
select.inp{cursor:pointer;appearance:none}
textarea.inp{resize:vertical;min-height:52px;line-height:1.4}

/* BUTTONS */
.btn{padding:7px 13px;border:1px solid;border-radius:4px;font-family:var(--font-d);font-size:.61rem;letter-spacing:.07em;cursor:pointer;transition:all .2s;white-space:nowrap}
.bg{background:transparent;border-color:#D4AF3750;color:var(--gold)}.bg:hover{background:#D4AF3712;border-color:var(--gold)}
.bgreen{background:transparent;border-color:#2ecc7140;color:var(--green)}.bgreen:hover{border-color:var(--green)}
.bred{background:transparent;border-color:#e74c3c40;color:var(--red)}.bred:hover{border-color:var(--red)}
.bgh{background:transparent;border-color:var(--border);color:var(--text-muted)}.bgh:hover{border-color:var(--border2);color:var(--text)}
.bsm{padding:4px 8px;font-size:.56rem}

/* COINS */
.cgrid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;margin-bottom:13px}
.cc{background:var(--bg3);border:1px solid var(--border);border-radius:6px;padding:12px 8px;text-align:center}
.cc.g{border-color:#D4AF3730}.cc.s{border-color:#C0C0C020}.cc.c{border-color:#B8733320}
.cs{width:26px;height:26px;border-radius:50%;margin:0 auto 7px;display:flex;align-items:center;justify-content:center;font-family:var(--font-d);font-size:.6rem;font-weight:700}
.cs.g{background:var(--gold);color:#2a1a00;box-shadow:0 0 10px #D4AF3750}
.cs.s{background:var(--silver);color:#1a1a1a}
.cs.c{background:var(--copper);color:#1a0a00}
.cv{font-family:var(--font-d);font-size:1.3rem;font-weight:600;line-height:1;margin-bottom:2px}
.cc.g .cv{color:var(--gold)}.cc.s .cv{color:#ccc}.cc.c .cv{color:#d07040}
.cl{font-size:.62rem;color:var(--text-muted);letter-spacing:.1em}
.cctrl{display:flex;gap:3px;margin-top:8px}
.cbtn{flex:1;padding:4px;background:var(--bg4);border:1px solid var(--border);border-radius:3px;color:var(--text-muted);font-size:.88rem;cursor:pointer;transition:all .15s;line-height:1}
.cbtn:hover{background:var(--bg2);color:var(--gold);border-color:var(--gold-glow)}
.tbar{background:var(--bg2);border:1px solid var(--border);border-radius:5px;padding:10px 14px;display:flex;justify-content:space-between;align-items:center;margin-bottom:14px}
.tlbl{font-size:.69rem;color:var(--text-muted);letter-spacing:.06em}
.tgp{font-family:var(--font-d);font-size:.9rem;color:var(--gold)}

/* LOG */
.logw{max-height:175px;overflow-y:auto}
.le{display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid #1e1a0e;gap:8px;font-size:.8rem}
.le:last-child{border:none}
.ld{color:#b0a080;flex:1}
.la{font-family:var(--font-d);font-size:.72rem}
.la.earn{color:var(--green)}.la.spend{color:var(--red)}
.lt2{font-size:.63rem;color:var(--text-muted)}

/* QUEST */
.q-active{color:#f39c12;border-color:#f39c1240}.q-completed{color:var(--green);border-color:#2ecc7140}
.q-failed{color:var(--red);border-color:#e74c3c30}.q-main{color:#9b59b6;border-color:#9b59b640}
.q-side{color:#3498db;border-color:#3498db40}.q-bounty{color:#e67e22;border-color:#e67e2240}
.q-rumor{color:var(--text-muted);border-color:var(--border)}

/* NPC */
.d-friendly{color:var(--green);border-color:#2ecc7140}.d-neutral{color:var(--text-dim);border-color:var(--border)}
.d-hostile{color:var(--red);border-color:#e74c3c40}.d-unknown{color:var(--text-muted);border-color:var(--border)}
.d-deceased{color:#666;border-color:#44444440;text-decoration:line-through}

/* LOCATION */
.l-visited{color:var(--green);border-color:#2ecc7140}.l-known{color:#3498db;border-color:#3498db40}
.l-rumored{color:var(--text-dim);border-color:var(--border)}.l-dangerous{color:var(--red);border-color:#e74c3c40}

/* RARITY */
.r-common{color:#aaa;border-color:#aaa3}.r-uncommon{color:var(--green);border-color:#2ecc7140}
.r-rare{color:#3498db;border-color:#3498db40}.r-very{color:#9b59b6;border-color:#9b59b640}
.r-legendary{color:var(--orange);border-color:#f39c1240}

/* LOOT STATUS */
.ls-kept{color:var(--gold);border-color:#D4AF3740}.ls-sold{color:var(--green);border-color:#2ecc7140}
.ls-stored{color:#3498db;border-color:#3498db40}.ls-dropped{color:var(--text-muted);border-color:var(--border)}

/* AFFORD DOT */
.adot{width:8px;height:8px;border-radius:50%;flex-shrink:0}
.ay{background:var(--green);box-shadow:0 0 6px #2ecc7160}.an{background:#e74c3c40}.ad{background:var(--green)}

/* STATS */
.srow{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:13px}
.sbox{background:var(--bg3);border:1px solid var(--border);border-radius:5px;padding:10px;text-align:center}
.snum{font-family:var(--font-d);font-size:1.1rem;color:var(--gold)}
.slbl{font-size:.61rem;color:var(--text-muted);letter-spacing:.06em}

/* FILTERS */
.frow{display:flex;gap:5px;margin-bottom:11px;flex-wrap:wrap}
.fbtn{padding:3px 8px;background:transparent;border:1px solid var(--border);border-radius:20px;font-family:var(--font-d);font-size:.56rem;letter-spacing:.06em;color:var(--text-muted);cursor:pointer;transition:all .2s}
.fbtn:hover,.fbtn.on{border-color:var(--gold-dim);color:var(--gold);background:#D4AF3710}

/* MODALS */
.mover{display:none;position:fixed;inset:0;background:#000a;z-index:300;align-items:center;justify-content:center;padding:16px}
.mover.open{display:flex}
.modal{background:var(--bg2);border:1px solid var(--border2);border-radius:8px;padding:20px;width:100%;max-width:460px;max-height:88vh;overflow-y:auto;box-shadow:0 20px 60px #000c}
.mtitle{font-family:var(--font-d);font-size:.83rem;color:var(--gold);letter-spacing:.08em;margin-bottom:14px;padding-bottom:10px;border-bottom:1px solid var(--border)}
.mfoot{display:flex;gap:8px;margin-top:13px;padding-top:11px;border-top:1px solid var(--border)}

::-webkit-scrollbar{width:4px;height:4px}
::-webkit-scrollbar-track{background:var(--bg2)}
::-webkit-scrollbar-thumb{background:var(--border2);border-radius:2px}
</style>
</head>
<body>

<!-- SAVE FLASH -->
<div id="save-flash">✦ Saved</div>

<!-- HEADER -->
<div class="hdr">
  <h1>⚔ Adventurer's Ledger</h1>
  <p class="hdr-sub">Campaign tracker — wealth · inventory · loot · upgrades · npcs · locations · quests</p>
</div>

<!-- PROFILE BAR -->
<div class="prof-bar">
  <span class="prof-icon" id="prof-icon">🧙</span>
  <span class="prof-lbl">Profile:</span>
  <div class="prof-wrap">
    <select class="prof-sel" id="prof-sel" onchange="switchProfile(parseInt(this.value))"></select>
  </div>
  <button class="pbtn" onclick="openEditProfile()">✎ Edit</button>
  <button class="pbtn g" onclick="openM('m-newprof')">+ New</button>
  <button class="pbtn r" id="delbtn" onclick="deleteProfile()">🗑</button>
</div>

<!-- TABS -->
<div class="tab-bar">
  <button class="tab-btn active" onclick="goTab('treasury')">💰 Treasury</button>
  <button class="tab-btn" onclick="goTab('inventory')">🎒 Inventory</button>
  <button class="tab-btn" onclick="goTab('loot')">💎 Loot</button>
  <button class="tab-btn" onclick="goTab('upgrades')">✦ Upgrades</button>
  <button class="tab-btn" onclick="goTab('npcs')">👤 NPCs</button>
  <button class="tab-btn" onclick="goTab('locations')">🗺 Locations</button>
  <button class="tab-btn" onclick="goTab('quests')">📜 Quests</button>
</div>

<!-- ═══ TREASURY ═══ -->
<div id="p-treasury" class="panel active">
  <p class="sec">Current Wealth</p>
  <div class="cgrid">
    <div class="cc g"><div class="cs g">G</div><div class="cv" id="vg">0</div><div class="cl">GOLD</div><div class="cctrl"><button class="cbtn" onclick="adjCoin('gold',-1)">−</button><button class="cbtn" onclick="adjCoin('gold',1)">+</button></div></div>
    <div class="cc s"><div class="cs s">S</div><div class="cv" id="vs">0</div><div class="cl">SILVER</div><div class="cctrl"><button class="cbtn" onclick="adjCoin('silver',-1)">−</button><button class="cbtn" onclick="adjCoin('silver',1)">+</button></div></div>
    <div class="cc c"><div class="cs c">C</div><div class="cv" id="vc">0</div><div class="cl">COPPER</div><div class="cctrl"><button class="cbtn" onclick="adjCoin('copper',-1)">−</button><button class="cbtn" onclick="adjCoin('copper',1)">+</button></div></div>
  </div>
  <div class="tbar"><span class="tlbl">⚖ Total Value</span><span class="tgp" id="tgp">0.00 gp</span></div>
  <p class="sec">Record Transaction</p>
  <div class="fbox">
    <div class="fr"><input class="inp f1" id="txdesc" placeholder="Description (e.g. Sold dragon hide)"/></div>
    <div class="fr">
      <input class="inp sm" id="txg" type="number" min="0" placeholder="GP"/>
      <input class="inp sm" id="txs" type="number" min="0" placeholder="SP"/>
      <input class="inp sm" id="txc" type="number" min="0" placeholder="CP"/>
      <button class="btn bgreen" onclick="doTxn('earn')">+ Earn</button>
      <button class="btn bred" onclick="doTxn('spend')">− Spend</button>
    </div>
  </div>
  <p class="sec">Transaction Log</p>
  <div class="logw" id="loglist"><div class="empty">No transactions yet.</div></div>
</div>

<!-- ═══ INVENTORY ═══ -->
<div id="p-inventory" class="panel">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
    <p class="sec" style="margin:0;border:none;padding:0">Carried Items</p>
    <button class="btn bg bsm" onclick="openM('m-item')">+ Add Item</button>
  </div>
  <div class="frow" id="inv-filters">
    <button class="fbtn on" onclick="flt('inv','All',this)">All</button>
    <button class="fbtn" onclick="flt('inv','Common',this)">Common</button>
    <button class="fbtn" onclick="flt('inv','Uncommon',this)">Uncommon</button>
    <button class="fbtn" onclick="flt('inv','Rare',this)">Rare</button>
    <button class="fbtn" onclick="flt('inv','Very Rare',this)">Very Rare</button>
    <button class="fbtn" onclick="flt('inv','Legendary',this)">Legendary</button>
  </div>
  <div id="invlist"><div class="empty">Your pack is empty, traveller.</div></div>
</div>

<!-- ═══ LOOT ═══ -->
<div id="p-loot" class="panel">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
    <p class="sec" style="margin:0;border:none;padding:0">Loot Tracker</p>
    <button class="btn bg bsm" onclick="openM('m-loot')">+ Add Loot</button>
  </div>
  <div class="srow">
    <div class="sbox"><div class="snum" id="lt-n">0</div><div class="slbl">TOTAL</div></div>
    <div class="sbox"><div class="snum" id="lt-k">0</div><div class="slbl">KEPT</div></div>
    <div class="sbox"><div class="snum" id="lt-v">0gp</div><div class="slbl">EST. VALUE</div></div>
  </div>
  <div class="frow">
    <button class="fbtn on" onclick="flt('loot','All',this)">All</button>
    <button class="fbtn" onclick="flt('loot','Kept',this)">Kept</button>
    <button class="fbtn" onclick="flt('loot','Sold',this)">Sold</button>
    <button class="fbtn" onclick="flt('loot','Stored',this)">Stored</button>
    <button class="fbtn" onclick="flt('loot','Dropped',this)">Dropped</button>
  </div>
  <div id="lootlist"><div class="empty">No loot recorded yet.</div></div>
</div>

<!-- ═══ UPGRADES ═══ -->
<div id="p-upgrades" class="panel">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px">
    <p class="sec" style="margin:0;border:none;padding:0">Wishlist &amp; Goals</p>
    <button class="btn bg bsm" onclick="openM('m-upg')">+ Add Goal</button>
  </div>
  <div class="tbar" style="margin-top:10px"><span class="tlbl">Treasury</span><span class="tgp" id="utreas">0.00 gp</span></div>
  <div id="upglist"><div class="empty">No goals set yet — what do you seek?</div></div>
</div>

<!-- ═══ NPCS ═══ -->
<div id="p-npcs" class="panel">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
    <p class="sec" style="margin:0;border:none;padding:0">NPC Roster</p>
    <button class="btn bg bsm" onclick="openM('m-npc')">+ Add NPC</button>
  </div>
  <div class="frow">
    <button class="fbtn on" onclick="flt('npc','All',this)">All</button>
    <button class="fbtn" onclick="flt('npc','Friendly',this)">Friendly</button>
    <button class="fbtn" onclick="flt('npc','Neutral',this)">Neutral</button>
    <button class="fbtn" onclick="flt('npc','Hostile',this)">Hostile</button>
    <button class="fbtn" onclick="flt('npc','Unknown',this)">Unknown</button>
    <button class="fbtn" onclick="flt('npc','Deceased',this)">Deceased</button>
  </div>
  <div id="npclist"><div class="empty">No NPCs recorded yet.</div></div>
</div>

<!-- ═══ LOCATIONS ═══ -->
<div id="p-locations" class="panel">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
    <p class="sec" style="margin:0;border:none;padding:0">Known Locations</p>
    <button class="btn bg bsm" onclick="openM('m-loc')">+ Add Location</button>
  </div>
  <div class="frow">
    <button class="fbtn on" onclick="flt('loc','All',this)">All</button>
    <button class="fbtn" onclick="flt('loc','Visited',this)">Visited</button>
    <button class="fbtn" onclick="flt('loc','Known',this)">Known</button>
    <button class="fbtn" onclick="flt('loc','Rumored',this)">Rumored</button>
    <button class="fbtn" onclick="flt('loc','Dangerous',this)">Dangerous</button>
    <button class="fbtn" onclick="flt('loc','City',this)">City</button>
    <button class="fbtn" onclick="flt('loc','Dungeon',this)">Dungeon</button>
    <button class="fbtn" onclick="flt('loc','Wilderness',this)">Wilderness</button>
  </div>
  <div id="loclist"><div class="empty">No locations recorded yet.</div></div>
</div>

<!-- ═══ QUESTS ═══ -->
<div id="p-quests" class="panel">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px">
    <p class="sec" style="margin:0;border:none;padding:0">Quest Log</p>
    <button class="btn bg bsm" onclick="openM('m-quest')">+ Add Quest</button>
  </div>
  <div class="srow">
    <div class="sbox"><div class="snum" id="qa">0</div><div class="slbl">ACTIVE</div></div>
    <div class="sbox"><div class="snum" id="qd">0</div><div class="slbl">COMPLETED</div></div>
    <div class="sbox"><div class="snum" id="qf">0</div><div class="slbl">FAILED</div></div>
  </div>
  <div class="frow">
    <button class="fbtn on" onclick="flt('quest','All',this)">All</button>
    <button class="fbtn" onclick="flt('quest','Active',this)">Active</button>
    <button class="fbtn" onclick="flt('quest','Completed',this)">Completed</button>
    <button class="fbtn" onclick="flt('quest','Failed',this)">Failed</button>
    <button class="fbtn" onclick="flt('quest','Main',this)">Main</button>
    <button class="fbtn" onclick="flt('quest','Side',this)">Side</button>
    <button class="fbtn" onclick="flt('quest','Bounty',this)">Bounty</button>
  </div>
  <div id="questlist"><div class="empty">No quests logged yet.</div></div>
</div>

<!-- ═══════════════ MODALS ═══════════════ -->
<div class="mover" id="m-newprof"><div class="modal">
  <div class="mtitle">🧙 Create New Profile</div>
  <div class="fr"><input class="inp" id="np-e" style="width:50px;text-align:center;font-size:1.2rem" placeholder="🧙" maxlength="2"/><input class="inp f1" id="np-n" placeholder="Character name"/></div>
  <div class="fr"><input class="inp fw" id="np-c" placeholder="Class / Campaign (e.g. Rogue · Dragon Campaign)"/></div>
  <div class="mfoot"><button class="btn bg" onclick="createProfile()">Create Profile</button><button class="btn bgh" onclick="closeM('m-newprof')">Cancel</button></div>
</div></div>

<div class="mover" id="m-editprof"><div class="modal">
  <div class="mtitle">✎ Edit Profile</div>
  <div class="fr"><input class="inp" id="ep-e" style="width:50px;text-align:center;font-size:1.2rem" maxlength="2"/><input class="inp f1" id="ep-n" placeholder="Character name"/></div>
  <div class="fr"><input class="inp fw" id="ep-c" placeholder="Class / Campaign"/></div>
  <div class="mfoot"><button class="btn bg" onclick="saveEditProfile()">Save Changes</button><button class="btn bgh" onclick="closeM('m-editprof')">Cancel</button></div>
</div></div>

<div class="mover" id="m-item"><div class="modal">
  <div class="mtitle">📦 Add Item</div>
  <div class="fr"><input class="inp" id="ii-e" style="width:50px;text-align:center;font-size:1.2rem" placeholder="⚔️" maxlength="2"/><input class="inp f1" id="ii-n" placeholder="Item name"/></div>
  <div class="fr"><select class="inp f1" id="ii-r"><option>Common</option><option>Uncommon</option><option>Rare</option><option>Very Rare</option><option>Legendary</option></select><input class="inp sm" id="ii-q" type="number" min="1" value="1"/></div>
  <div class="fr"><input class="inp f1" id="ii-w" placeholder="Weight (lb)"/><input class="inp f1" id="ii-v" placeholder="Value (gp)"/></div>
  <textarea class="inp fw" id="ii-no" placeholder="Notes, magical properties…"></textarea>
  <div class="mfoot"><button class="btn bg" onclick="addItem()">Add Item</button><button class="btn bgh" onclick="closeM('m-item')">Cancel</button></div>
</div></div>

<div class="mover" id="m-upg"><div class="modal">
  <div class="mtitle">✦ Add Goal</div>
  <div class="fr"><input class="inp fw" id="uu-n" placeholder="Item or upgrade name"/></div>
  <div class="fr"><input class="inp f1" id="uu-c" type="number" min="0" placeholder="Cost in GP"/><select class="inp f1" id="uu-p"><option>High</option><option selected>Medium</option><option>Low</option></select></div>
  <textarea class="inp fw" id="uu-no" placeholder="Notes, where to find it…"></textarea>
  <div class="mfoot"><button class="btn bg" onclick="addUpgrade()">Add Goal</button><button class="btn bgh" onclick="closeM('m-upg')">Cancel</button></div>
</div></div>

<div class="mover" id="m-quest"><div class="modal">
  <div class="mtitle">📜 Add Quest</div>
  <div class="fr"><input class="inp fw" id="qq-n" placeholder="Quest name"/></div>
  <div class="fr"><select class="inp f1" id="qq-t"><option>Main</option><option>Side</option><option>Bounty</option><option>Rumor</option></select><select class="inp f1" id="qq-s"><option>Active</option><option>Completed</option><option>Failed</option></select></div>
  <div class="fr"><input class="inp f1" id="qq-g" placeholder="Quest giver"/><input class="inp f1" id="qq-r" placeholder="Reward"/></div>
  <textarea class="inp fw" id="qq-d" placeholder="Objective / description…"></textarea>
  <textarea class="inp fw" id="qq-no" placeholder="Notes, clues, progress…" style="margin-top:7px"></textarea>
  <div class="mfoot"><button class="btn bg" onclick="addQuest()">Add Quest</button><button class="btn bgh" onclick="closeM('m-quest')">Cancel</button></div>
</div></div>

<div class="mover" id="m-npc"><div class="modal">
  <div class="mtitle">👤 Add NPC</div>
  <div class="fr"><input class="inp" id="nn-e" style="width:50px;text-align:center;font-size:1.2rem" placeholder="🧙" maxlength="2"/><input class="inp f1" id="nn-n" placeholder="NPC name"/></div>
  <div class="fr"><input class="inp fw" id="nn-r" placeholder="Role / class (e.g. Innkeeper, Wizard)"/></div>
  <div class="fr"><select class="inp f1" id="nn-d"><option>Friendly</option><option>Neutral</option><option>Hostile</option><option>Unknown</option><option>Deceased</option></select><input class="inp f1" id="nn-l" placeholder="Last known location"/></div>
  <textarea class="inp fw" id="nn-no" placeholder="Notes, secrets, relationship…"></textarea>
  <div class="mfoot"><button class="btn bg" onclick="addNpc()">Add NPC</button><button class="btn bgh" onclick="closeM('m-npc')">Cancel</button></div>
</div></div>

<div class="mover" id="m-loc"><div class="modal">
  <div class="mtitle">🗺 Add Location</div>
  <div class="fr"><input class="inp" id="ll-e" style="width:50px;text-align:center;font-size:1.2rem" placeholder="🏰" maxlength="2"/><input class="inp f1" id="ll-n" placeholder="Location name"/></div>
  <div class="fr"><select class="inp f1" id="ll-t"><option>City</option><option>Town</option><option>Village</option><option>Dungeon</option><option>Ruins</option><option>Wilderness</option><option>Castle</option><option>Temple</option><option>Cave</option><option>Other</option></select><select class="inp f1" id="ll-s"><option>Visited</option><option>Known</option><option>Rumored</option><option>Dangerous</option></select></div>
  <div class="fr"><input class="inp fw" id="ll-r" placeholder="Region / distance"/></div>
  <textarea class="inp fw" id="ll-d" placeholder="Description, notable features…"></textarea>
  <textarea class="inp fw" id="ll-no" placeholder="Notes, secrets, NPCs here…" style="margin-top:7px"></textarea>
  <div class="mfoot"><button class="btn bg" onclick="addLocation()">Add Location</button><button class="btn bgh" onclick="closeM('m-loc')">Cancel</button></div>
</div></div>

<div class="mover" id="m-loot"><div class="modal">
  <div class="mtitle">💎 Add Loot</div>
  <div class="fr"><input class="inp" id="lo-e" style="width:50px;text-align:center;font-size:1.2rem" placeholder="💎" maxlength="2"/><input class="inp f1" id="lo-n" placeholder="Item / loot name"/></div>
  <div class="fr"><select class="inp f1" id="lo-r"><option>Common</option><option>Uncommon</option><option>Rare</option><option>Very Rare</option><option>Legendary</option></select><input class="inp sm" id="lo-q" type="number" min="1" value="1"/></div>
  <div class="fr"><input class="inp f1" id="lo-f" placeholder="Source (e.g. Goblin Cave)"/><input class="inp f1" id="lo-v" type="number" min="0" placeholder="Value (gp)"/></div>
  <div class="fr"><select class="inp f1" id="lo-s"><option>Kept</option><option>Sold</option><option>Stored</option><option>Dropped</option></select></div>
  <textarea class="inp fw" id="lo-no" placeholder="Notes, magical properties…"></textarea>
  <div class="mfoot"><button class="btn bg" onclick="addLoot()">Add Loot</button><button class="btn bgh" onclick="closeM('m-loot')">Cancel</button></div>
</div></div>

<script>
// ═══════════════════════════════════════════
// STORAGE  — localStorage persistence
// ═══════════════════════════════════════════
const KEY = 'adv_ledger_v2';
let saveTimer = null;

function persist() {
  // Debounce: wait 300ms after last change before writing
  clearTimeout(saveTimer);
  saveTimer = setTimeout(function(){
    try {
      localStorage.setItem(KEY, JSON.stringify({ profiles: profiles, pi: pi, nxt: nxt }));
      flash();
    } catch(e) { console.warn('Save error:', e); }
  }, 300);
}

function loadSaved() {
  try {
    var raw = localStorage.getItem(KEY);
    if (!raw) return false;
    var d = JSON.parse(raw);
    if (d && d.profiles && d.profiles.length > 0) {
      profiles = d.profiles;
      pi = (typeof d.pi === 'number' && d.pi < d.profiles.length) ? d.pi : 0;
      nxt = (typeof d.nxt === 'number') ? d.nxt : 1000;
      return true;
    }
  } catch(e) { console.warn('Load error:', e); }
  return false;
}

function flash() {
  var el = document.getElementById('save-flash');
  el.style.opacity = '1';
  clearTimeout(el._ft);
  el._ft = setTimeout(function(){ el.style.opacity = '0'; }, 1400);
}

// ═══════════════════════════════════════════
// STATE
// ═══════════════════════════════════════════
var nxt = 1000;
function uid() { return nxt++; }

function blank() {
  return {
    gold:0, silver:0, copper:0, log:[],
    items:[], loot:[], upgrades:[],
    npcs:[], locs:[], quests:[],
    invF:'All', lootF:'All', npcF:'All', locF:'All', questF:'All'
  };
}

var profiles = [{ id:uid(), emoji:'🧙', name:'My Character', cls:'', data:blank() }];
var pi = 0;

function P() { return profiles[pi]; }
function D() { return profiles[pi].data; }

// ═══════════════════════════════════════════
// PROFILE MANAGEMENT
// ═══════════════════════════════════════════
function renderProfSel() {
  var sel = document.getElementById('prof-sel');
  sel.innerHTML = profiles.map(function(p, i) {
    return '<option value="' + i + '"' + (i===pi?' selected':'') + '>' +
           p.emoji + ' ' + p.name + (p.cls ? ' · ' + p.cls : '') + '</option>';
  }).join('');
  document.getElementById('prof-icon').textContent = P().emoji;
  var db = document.getElementById('delbtn');
  db.style.opacity = profiles.length > 1 ? '1' : '0.3';
  db.disabled = profiles.length <= 1;
}

function switchProfile(idx) {
  pi = idx;
  renderProfSel();
  renderAll();
}

function createProfile() {
  var name = g('np-n'); if (!name) return;
  profiles.push({ id:uid(), emoji:g('np-e')||'🧙', name:name, cls:g('np-c'), data:blank() });
  pi = profiles.length - 1;
  cl('np-n'); cl('np-c'); document.getElementById('np-e').value = '';
  closeM('m-newprof');
  renderProfSel(); renderAll(); persist();
}

function openEditProfile() {
  document.getElementById('ep-e').value = P().emoji;
  document.getElementById('ep-n').value = P().name;
  document.getElementById('ep-c').value = P().cls || '';
  openM('m-editprof');
}

function saveEditProfile() {
  var name = g('ep-n'); if (!name) return;
  P().name = name; P().emoji = g('ep-e')||'🧙'; P().cls = g('ep-c');
  closeM('m-editprof');
  renderProfSel(); persist();
}

function deleteProfile() {
  if (profiles.length <= 1) return;
  if (!confirm('Delete profile "' + P().name + '"? This cannot be undone.')) return;
  profiles.splice(pi, 1);
  pi = Math.max(0, pi - 1);
  renderProfSel(); renderAll(); persist();
}

// ═══════════════════════════════════════════
// TABS
// ═══════════════════════════════════════════
function goTab(id) {
  document.querySelectorAll('.tab-btn').forEach(function(b) {
    b.classList.toggle('active', b.getAttribute('onclick').indexOf("'"+id+"'") > -1);
  });
  document.querySelectorAll('.panel').forEach(function(p) { p.classList.remove('active'); });
  document.getElementById('p-' + id).classList.add('active');
}

// ═══════════════════════════════════════════
// MODALS
// ═══════════════════════════════════════════
function openM(id) { document.getElementById(id).classList.add('open'); }
function closeM(id) { document.getElementById(id).classList.remove('open'); }
document.querySelectorAll('.mover').forEach(function(m) {
  m.addEventListener('click', function(e) { if (e.target === m) m.classList.remove('open'); });
});

// ═══════════════════════════════════════════
// FILTERS
// ═══════════════════════════════════════════
var fltMap = { inv:'invF', loot:'lootF', npc:'npcF', loc:'locF', quest:'questF' };
var fltRender = { inv:renderItems, loot:renderLoot, npc:renderNpcs, loc:renderLocs, quest:renderQuests };

function flt(type, val, btn) {
  D()[fltMap[type]] = val;
  btn.closest('.frow').querySelectorAll('.fbtn').forEach(function(b){ b.classList.remove('on'); });
  btn.classList.add('on');
  fltRender[type]();
}

// ═══════════════════════════════════════════
// TREASURY
// ═══════════════════════════════════════════
function tCopper() { return D().gold*100 + D().silver*10 + D().copper; }
function gpStr()   { return (tCopper()/100).toFixed(2); }

function adjCoin(type, delta) {
  var mult = { gold:100, silver:10, copper:1 };
  var t = Math.max(0, tCopper() + delta * mult[type]);
  D().gold = Math.floor(t/100);
  D().silver = Math.floor((t%100)/10);
  D().copper = t % 10;
  renderCoins(); persist();
}

function renderCoins() {
  document.getElementById('vg').textContent = D().gold;
  document.getElementById('vs').textContent = D().silver;
  document.getElementById('vc').textContent = D().copper;
  document.getElementById('tgp').textContent = gpStr() + ' gp';
  document.getElementById('utreas').textContent = gpStr() + ' gp';
}

function doTxn(type) {
  var gv = parseInt(document.getElementById('txg').value)||0;
  var sv = parseInt(document.getElementById('txs').value)||0;
  var cv = parseInt(document.getElementById('txc').value)||0;
  if (!gv && !sv && !cv) return;
  var desc = document.getElementById('txdesc').value.trim() || (type==='earn' ? 'Gold earned' : 'Purchase');
  var delta = (type==='earn' ? 1 : -1) * (gv*100 + sv*10 + cv);
  var t = Math.max(0, tCopper() + delta);
  D().gold = Math.floor(t/100); D().silver = Math.floor((t%100)/10); D().copper = t%10;
  D().log.unshift({ id:uid(), type:type, desc:desc, g:gv, s:sv, c:cv,
    time: new Date().toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'}) });
  if (D().log.length > 60) D().log.pop();
  document.getElementById('txdesc').value = '';
  document.getElementById('txg').value = '';
  document.getElementById('txs').value = '';
  document.getElementById('txc').value = '';
  renderCoins(); renderLog(); persist();
}

function renderLog() {
  var el = document.getElementById('loglist');
  if (!D().log.length) { el.innerHTML = '<div class="empty">No transactions yet.</div>'; return; }
  el.innerHTML = D().log.map(function(e) {
    return '<div class="le"><span class="ld">' + esc(e.desc) + '</span>' +
      '<span class="la ' + e.type + '">' + (e.type==='earn'?'+':'−') +
      (e.g?' '+e.g+'gp':'') + (e.s?' '+e.s+'sp':'') + (e.c?' '+e.c+'cp':'') + '</span>' +
      '<span class="lt2">' + e.time + '</span></div>';
  }).join('');
}

// ═══════════════════════════════════════════
// INVENTORY
// ═══════════════════════════════════════════
function addItem() {
  var name = g('ii-n'); if (!name) return;
  D().items.push({ id:uid(), emoji:g('ii-e')||'📦', name:name, rarity:g('ii-r'),
    qty:parseInt(g('ii-q'))||1, weight:g('ii-w'), value:g('ii-v'), notes:g('ii-no') });
  cl('ii-n'); cl('ii-w'); cl('ii-v'); cl('ii-no');
  document.getElementById('ii-e').value=''; document.getElementById('ii-q').value=1;
  closeM('m-item'); renderItems(); persist();
}
function adjQty(id, d) {
  D().items = D().items.map(function(i){ return i.id===id ? Object.assign({},i,{qty:Math.max(0,i.qty+d)}) : i; })
    .filter(function(i){ return i.qty > 0; });
  renderItems(); persist();
}
function delItem(id) { D().items = D().items.filter(function(i){ return i.id!==id; }); renderItems(); persist(); }

function renderItems() {
  var el = document.getElementById('invlist');
  var f = D().invF;
  var list = f==='All' ? D().items : D().items.filter(function(i){ return i.rarity===f; });
  if (!list.length) { el.innerHTML = '<div class="empty">' + (D().items.length?'No items match filter.':'Your pack is empty, traveller.') + '</div>'; return; }
  el.innerHTML = list.map(function(i) {
    return '<div class="card"><div class="card-row">' +
      '<div class="ci">' + i.emoji + '</div>' +
      '<div class="cb"><div class="cn">' + esc(i.name) + '</div>' +
      '<div class="cm"><span class="badge ' + rc(i.rarity) + '">' + i.rarity + '</span>' +
      (i.weight ? '<span>⚖ ' + esc(i.weight) + ' lb</span>' : '') +
      (i.value ? '<span style="color:var(--gold-dim)">◈ ' + esc(i.value) + ' gp</span>' : '') +
      '</div>' + (i.notes ? '<div class="cd">' + esc(i.notes) + '</div>' : '') + '</div>' +
      '<div class="ca"><div style="display:flex;align-items:center;gap:4px">' +
      '<button class="cbtn" style="width:22px;height:22px;padding:0" onclick="adjQty(' + i.id + ',-1)">−</button>' +
      '<span style="font-family:var(--font-d);font-size:.85rem;color:var(--gold);min-width:18px;text-align:center">' + i.qty + '</span>' +
      '<button class="cbtn" style="width:22px;height:22px;padding:0" onclick="adjQty(' + i.id + ',1)">+</button></div>' +
      '<button class="btn bgh bsm" onclick="delItem(' + i.id + ')" style="color:var(--red);border-color:#e74c3c30">✕</button>' +
      '</div></div></div>';
  }).join('');
}

// ═══════════════════════════════════════════
// LOOT
// ═══════════════════════════════════════════
function addLoot() {
  var name = g('lo-n'); if (!name) return;
  D().loot.push({ id:uid(), emoji:g('lo-e')||'💎', name:name, rarity:g('lo-r'),
    qty:parseInt(g('lo-q'))||1, from:g('lo-f'), value:parseFloat(g('lo-v'))||0,
    status:g('lo-s'), notes:g('lo-no') });
  cl('lo-n'); cl('lo-f'); cl('lo-v'); cl('lo-no');
  document.getElementById('lo-e').value=''; document.getElementById('lo-q').value=1;
  closeM('m-loot'); renderLoot(); persist();
}
function setLootSt(id, st) { D().loot = D().loot.map(function(l){ return l.id===id?Object.assign({},l,{status:st}):l; }); renderLoot(); persist(); }
function delLoot(id)  { D().loot = D().loot.filter(function(l){ return l.id!==id; }); renderLoot(); persist(); }

function renderLoot() {
  var tot = D().loot.length;
  var kept = D().loot.filter(function(l){ return l.status==='Kept'; }).length;
  var val = D().loot.reduce(function(a,l){ return a + l.value*l.qty; }, 0);
  document.getElementById('lt-n').textContent = tot;
  document.getElementById('lt-k').textContent = kept;
  document.getElementById('lt-v').textContent = val>999 ? Math.round(val/100)/10+'k gp' : val+'gp';
  var el = document.getElementById('lootlist');
  var f = D().lootF;
  var list = f==='All' ? D().loot : D().loot.filter(function(l){ return l.status===f; });
  if (!list.length) { el.innerHTML = '<div class="empty">' + (D().loot.length?'No loot matches filter.':'No loot recorded yet.') + '</div>'; return; }
  el.innerHTML = list.map(function(l) {
    return '<div class="card"><div class="card-row">' +
      '<div class="ci">' + l.emoji + '</div>' +
      '<div class="cb"><div class="cn">' + esc(l.name) + (l.qty>1?'<span style="color:var(--text-muted);font-size:.76rem"> ×'+l.qty+'</span>':'') + '</div>' +
      '<div class="cm"><span class="badge ' + rc(l.rarity) + '">' + l.rarity + '</span>' +
      '<span class="badge ls-' + l.status.toLowerCase() + '">' + l.status + '</span>' +
      (l.from ? '<span>📌 ' + esc(l.from) + '</span>' : '') +
      (l.value ? '<span style="color:var(--gold-dim)">◈ ' + (l.value*l.qty) + ' gp</span>' : '') +
      '</div>' + (l.notes ? '<div class="cd">' + esc(l.notes) + '</div>' : '') + '</div>' +
      '<div class="ca" style="flex-direction:column;gap:4px">' +
      '<select class="inp" style="padding:3px 5px;font-size:.6rem;width:70px" onchange="setLootSt(' + l.id + ',this.value)">' +
      ['Kept','Sold','Stored','Dropped'].map(function(s){ return '<option'+(s===l.status?' selected':'')+'>'+s+'</option>'; }).join('') +
      '</select><button class="btn bgh bsm" onclick="delLoot(' + l.id + ')" style="color:var(--text-muted)">🗑</button>' +
      '</div></div></div>';
  }).join('');
}

// ═══════════════════════════════════════════
// UPGRADES
// ═══════════════════════════════════════════
function addUpgrade() {
  var name = g('uu-n'); if (!name) return;
  D().upgrades.push({ id:uid(), name:name, cost:parseFloat(g('uu-c'))||0, priority:g('uu-p'), notes:g('uu-no'), acquired:false });
  cl('uu-n'); cl('uu-c'); cl('uu-no');
  closeM('m-upg'); renderUpgrades(); persist();
}
function togAcq(id) { D().upgrades = D().upgrades.map(function(u){ return u.id===id?Object.assign({},u,{acquired:!u.acquired}):u; }); renderUpgrades(); persist(); }
function delUpg(id) { D().upgrades = D().upgrades.filter(function(u){ return u.id!==id; }); renderUpgrades(); persist(); }

var PORD = { High:0, Medium:1, Low:2 };
function renderUpgrades() {
  document.getElementById('utreas').textContent = gpStr() + ' gp';
  var el = document.getElementById('upglist');
  if (!D().upgrades.length) { el.innerHTML = '<div class="empty">No goals set yet — what do you seek?</div>'; return; }
  var sorted = D().upgrades.slice().sort(function(a,b){
    if (a.acquired !== b.acquired) return a.acquired ? 1 : -1;
    return PORD[a.priority] - PORD[b.priority];
  });
  var pCol = { High:'var(--red)', Medium:'var(--orange)', Low:'var(--text-muted)' };
  var pBor = { High:'#e74c3c40', Medium:'#f39c1240', Low:'var(--border)' };
  el.innerHTML = sorted.map(function(u) {
    var afford = parseFloat(gpStr()) >= u.cost;
    return '<div class="card" style="' + (u.acquired?'opacity:.6':afford?'border-color:#D4AF3730':'') + '">' +
      '<div class="card-row" style="align-items:center">' +
      '<div class="adot ' + (u.acquired?'ad':afford?'ay':'an') + '" title="' + (u.acquired?'Acquired':afford?'Can afford':'Need more gold') + '"></div>' +
      '<div class="cb"><div class="cn" style="' + (u.acquired?'text-decoration:line-through':'') + '">' + esc(u.name) + '</div>' +
      '<div class="cm"><span class="badge" style="color:' + pCol[u.priority] + ';border-color:' + pBor[u.priority] + '">' + u.priority + '</span>' +
      (u.notes ? '<span style="font-style:italic">' + esc(u.notes) + '</span>' : '') + '</div></div>' +
      '<div class="ca" style="flex-direction:column;align-items:flex-end;gap:4px">' +
      '<span style="font-family:var(--font-d);font-size:.8rem;color:var(--gold)">' + (u.cost?u.cost+' gp':'—') + '</span>' +
      '<div style="display:flex;gap:4px">' +
      '<button class="btn bgh bsm" onclick="togAcq(' + u.id + ')">' + (u.acquired?'↩':'✓') + '</button>' +
      '<button class="btn bgh bsm" onclick="delUpg(' + u.id + ')" style="color:var(--red);border-color:#e74c3c30">✕</button>' +
      '</div></div></div></div>';
  }).join('');
}

// ═══════════════════════════════════════════
// NPCS
// ═══════════════════════════════════════════
function addNpc() {
  var name = g('nn-n'); if (!name) return;
  D().npcs.push({ id:uid(), emoji:g('nn-e')||'👤', name:name, role:g('nn-r'), disp:g('nn-d'), loc:g('nn-l'), notes:g('nn-no') });
  cl('nn-n'); cl('nn-r'); cl('nn-l'); cl('nn-no');
  document.getElementById('nn-e').value = '';
  closeM('m-npc'); renderNpcs(); persist();
}
function setDisp(id, disp) { D().npcs = D().npcs.map(function(n){ return n.id===id?Object.assign({},n,{disp:disp}):n; }); renderNpcs(); persist(); }
function delNpc(id)  { D().npcs = D().npcs.filter(function(n){ return n.id!==id; }); renderNpcs(); persist(); }

function renderNpcs() {
  var el = document.getElementById('npclist');
  var f = D().npcF;
  var list = f==='All' ? D().npcs : D().npcs.filter(function(n){ return n.disp===f; });
  if (!list.length) { el.innerHTML = '<div class="empty">' + (D().npcs.length?'No NPCs match filter.':'No NPCs recorded yet.') + '</div>'; return; }
  el.innerHTML = list.map(function(n) {
    return '<div class="card"><div class="card-row">' +
      '<div class="ci">' + n.emoji + '</div>' +
      '<div class="cb"><div class="cn">' + esc(n.name) + '</div>' +
      '<div class="cm"><span class="badge d-' + n.disp.toLowerCase() + '">' + n.disp + '</span>' +
      (n.role ? '<span>' + esc(n.role) + '</span>' : '') +
      (n.loc  ? '<span>📍 ' + esc(n.loc) + '</span>' : '') +
      '</div>' + (n.notes ? '<div class="cd">' + esc(n.notes) + '</div>' : '') + '</div>' +
      '<div class="ca" style="flex-direction:column;gap:4px">' +
      '<select class="inp" style="padding:3px 5px;font-size:.6rem;width:82px" onchange="setDisp(' + n.id + ',this.value)">' +
      ['Friendly','Neutral','Hostile','Unknown','Deceased'].map(function(d){ return '<option'+(d===n.disp?' selected':'')+'>'+d+'</option>'; }).join('') +
      '</select><button class="btn bgh bsm" onclick="delNpc(' + n.id + ')" style="color:var(--text-muted)">🗑</button>' +
      '</div></div></div>';
  }).join('');
}

// ═══════════════════════════════════════════
// LOCATIONS
// ═══════════════════════════════════════════
function addLocation() {
  var name = g('ll-n'); if (!name) return;
  D().locs.push({ id:uid(), emoji:g('ll-e')||'📍', name:name, type:g('ll-t'), status:g('ll-s'), region:g('ll-r'), desc:g('ll-d'), notes:g('ll-no') });
  cl('ll-n'); cl('ll-r'); cl('ll-d'); cl('ll-no');
  document.getElementById('ll-e').value = '';
  closeM('m-loc'); renderLocs(); persist();
}
function setLocSt(id, st) { D().locs = D().locs.map(function(l){ return l.id===id?Object.assign({},l,{status:st}):l; }); renderLocs(); persist(); }
function delLoc(id) { D().locs = D().locs.filter(function(l){ return l.id!==id; }); renderLocs(); persist(); }

function renderLocs() {
  var el = document.getElementById('loclist');
  var f = D().locF;
  var list = D().locs;
  if (['City','Dungeon','Wilderness'].indexOf(f) > -1) list = list.filter(function(l){ return l.type===f; });
  else if (f !== 'All') list = list.filter(function(l){ return l.status===f; });
  if (!list.length) { el.innerHTML = '<div class="empty">' + (D().locs.length?'No locations match filter.':'No locations recorded yet.') + '</div>'; return; }
  el.innerHTML = list.map(function(l) {
    return '<div class="card"><div class="card-row">' +
      '<div class="ci">' + l.emoji + '</div>' +
      '<div class="cb"><div class="cn">' + esc(l.name) + '</div>' +
      '<div class="cm"><span class="badge l-' + l.status.toLowerCase() + '">' + l.status + '</span>' +
      '<span class="badge" style="color:var(--text-dim);border-color:var(--border)">' + l.type + '</span>' +
      (l.region ? '<span>🧭 ' + esc(l.region) + '</span>' : '') +
      '</div>' + (l.desc ? '<div class="cd">' + esc(l.desc) + '</div>' : '') +
      (l.notes ? '<div class="cd" style="border-top:none;padding-top:0;margin-top:2px;color:var(--text-muted)">📝 ' + esc(l.notes) + '</div>' : '') +
      '</div><div class="ca" style="flex-direction:column;gap:4px">' +
      '<select class="inp" style="padding:3px 5px;font-size:.6rem;width:86px" onchange="setLocSt(' + l.id + ',this.value)">' +
      ['Visited','Known','Rumored','Dangerous'].map(function(s){ return '<option'+(s===l.status?' selected':'')+'>'+s+'</option>'; }).join('') +
      '</select><button class="btn bgh bsm" onclick="delLoc(' + l.id + ')" style="color:var(--text-muted)">🗑</button>' +
      '</div></div></div>';
  }).join('');
}

// ═══════════════════════════════════════════
// QUESTS
// ═══════════════════════════════════════════
function addQuest() {
  var name = g('qq-n'); if (!name) return;
  D().quests.push({ id:uid(), name:name, type:g('qq-t'), status:g('qq-s'), giver:g('qq-g'), reward:g('qq-r'), desc:g('qq-d'), notes:g('qq-no') });
  cl('qq-n'); cl('qq-g'); cl('qq-r'); cl('qq-d'); cl('qq-no');
  closeM('m-quest'); renderQuests(); persist();
}
function setQSt(id, st) { D().quests = D().quests.map(function(q){ return q.id===id?Object.assign({},q,{status:st}):q; }); renderQuests(); persist(); }
function delQuest(id) { D().quests = D().quests.filter(function(q){ return q.id!==id; }); renderQuests(); persist(); }

var QI = { Main:'⚔', Side:'◈', Bounty:'⚡', Rumor:'?' };
var QORD = { Active:0, Failed:2, Completed:3 };
function renderQuests() {
  document.getElementById('qa').textContent = D().quests.filter(function(q){ return q.status==='Active'; }).length;
  document.getElementById('qd').textContent = D().quests.filter(function(q){ return q.status==='Completed'; }).length;
  document.getElementById('qf').textContent = D().quests.filter(function(q){ return q.status==='Failed'; }).length;
  var el = document.getElementById('questlist');
  var flt = D().questF;
  var list = D().quests.slice();
  if (['Main','Side','Bounty'].indexOf(flt) > -1) list = list.filter(function(q){ return q.type===flt; });
  else if (flt !== 'All') list = list.filter(function(q){ return q.status===flt; });
  list.sort(function(a,b){ return (QORD[a.status]||0)-(QORD[b.status]||0); });
  if (!list.length) { el.innerHTML = '<div class="empty">' + (D().quests.length?'No quests match filter.':'No quests logged yet.') + '</div>'; return; }
  el.innerHTML = list.map(function(q) {
    return '<div class="card"><div class="card-row">' +
      '<div class="ci" style="font-size:.8rem;font-family:var(--font-d);color:var(--gold)">' + (QI[q.type]||'?') + '</div>' +
      '<div class="cb"><div class="cn">' + esc(q.name) + '</div>' +
      '<div class="cm"><span class="badge q-' + q.type.toLowerCase() + '">' + q.type + '</span>' +
      '<span class="badge q-' + q.status.toLowerCase() + '">' + q.status + '</span>' +
      (q.giver  ? '<span>📌 ' + esc(q.giver)  + '</span>' : '') +
      (q.reward ? '<span style="color:var(--gold-dim)">⚖ ' + esc(q.reward) + '</span>' : '') +
      '</div>' + (q.desc  ? '<div class="cd">' + esc(q.desc)  + '</div>' : '') +
      (q.notes ? '<div class="cd" style="border-top:none;padding-top:0;margin-top:2px;color:var(--text-muted)">📝 ' + esc(q.notes) + '</div>' : '') +
      '</div><div class="ca" style="flex-direction:column;gap:4px">' +
      (q.status!=='Completed' ? '<button class="btn bgreen bsm" onclick="setQSt(' + q.id + ',\'Completed\')">✓</button>' : '') +
      (q.status==='Active'    ? '<button class="btn bgh bsm" onclick="setQSt(' + q.id + ',\'Failed\')" style="color:var(--red);border-color:#e74c3c30">✗</button>' : '') +
      (q.status!=='Active'    ? '<button class="btn bgh bsm" onclick="setQSt(' + q.id + ',\'Active\')">↩</button>' : '') +
      '<button class="btn bgh bsm" onclick="delQuest(' + q.id + ')" style="color:var(--text-muted)">🗑</button>' +
      '</div></div></div>';
  }).join('');
}

// ═══════════════════════════════════════════
// RENDER ALL
// ═══════════════════════════════════════════
function renderAll() {
  renderCoins(); renderLog(); renderItems();
  renderLoot(); renderUpgrades();
  renderNpcs(); renderLocs(); renderQuests();
  // Reset all filter buttons to first (All)
  document.querySelectorAll('.frow').forEach(function(row){
    row.querySelectorAll('.fbtn').forEach(function(b,i){ b.classList.toggle('on', i===0); });
  });
  var d = D();
  d.invF='All'; d.lootF='All'; d.npcF='All'; d.locF='All'; d.questF='All';
}

// ═══════════════════════════════════════════
// HELPERS
// ═══════════════════════════════════════════
function g(id)  { return document.getElementById(id).value.trim(); }
function cl(id) { document.getElementById(id).value = ''; }
function esc(s) { var d=document.createElement('div'); d.textContent=s||''; return d.innerHTML; }
function rc(r)  { return ({'Common':'r-common','Uncommon':'r-uncommon','Rare':'r-rare','Very Rare':'r-very','Legendary':'r-legendary'})[r]||'r-common'; }

// ═══════════════════════════════════════════
// BOOT
// ═══════════════════════════════════════════
loadSaved();
renderProfSel();
renderAll();
</script>
</body>
</html>
