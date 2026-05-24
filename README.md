<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>LOOK — Compra. Vende. Reutiliza.</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@3.2.0/tabler-icons.min.css" />
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,300&family=Playfair+Display:wght@400;500&display=swap" rel="stylesheet" />
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
    :root {
      --green: #6BB58C; --green-light: #E8F5EF; --green-dark: #4a8a68;
      --navy: #0D2340; --navy-light: #1a3a5c;
      --bg: #F8F9FA; --surface: #FFFFFF;
      --text: #0D2340; --text-muted: #7A8A99; --border: #E8ECF0;
      --radius: 16px; --radius-sm: 10px;
      --shadow: 0 2px 16px rgba(13,35,64,0.08); --shadow-lg: 0 8px 32px rgba(13,35,64,0.14);
    }
    html, body { height: 100%; overflow: hidden; background: #111827; font-family: 'DM Sans', sans-serif; }
    #app-wrap { display: flex; justify-content: center; align-items: center; height: 100vh; }
    #app {
      width: 100%; max-width: 420px; height: 100vh; max-height: 850px;
      background: var(--bg); border-radius: 0; overflow: hidden;
      position: relative; display: flex; flex-direction: column;
      box-shadow: 0 30px 80px rgba(0,0,0,0.5);
    }
    @media (min-width: 480px) { #app { border-radius: 36px; height: 850px; } }

    /* SCREENS */
    .screen { position: absolute; inset: 0; display: none; flex-direction: column; background: var(--bg); overflow: hidden; }
    .screen.active { display: flex; }
    .scroll-area { flex: 1; overflow-y: auto; -webkit-overflow-scrolling: touch; scrollbar-width: none; }
    .scroll-area::-webkit-scrollbar { display: none; }

    /* STATUS BAR */
    .status-bar { height: 44px; background: var(--surface); display: flex; align-items: center; justify-content: space-between; padding: 0 20px; flex-shrink: 0; }
    .status-time { font-size: 15px; font-weight: 600; color: var(--text); }
    .status-icons { display: flex; gap: 6px; align-items: center; }
    .status-icons i { font-size: 14px; color: var(--text); }

    /* BOTTOM NAV */
    .bottom-nav { height: 72px; background: var(--surface); border-top: 1px solid var(--border); display: flex; align-items: center; justify-content: space-around; padding: 0 8px 8px; flex-shrink: 0; }
    .nav-item { display: flex; flex-direction: column; align-items: center; gap: 3px; cursor: pointer; padding: 8px 12px; border-radius: 12px; transition: all 0.2s; flex: 1; }
    .nav-item.active .nav-icon { color: var(--green); }
    .nav-item.active .nav-label { color: var(--green); font-weight: 500; }
    .nav-icon { font-size: 22px; color: var(--text-muted); transition: color 0.2s; }
    .nav-label { font-size: 10px; color: var(--text-muted); }
    .nav-ai { background: var(--navy); border-radius: 14px; padding: 10px 16px !important; }
    .nav-ai .nav-icon { color: white !important; font-size: 18px; }
    .nav-ai .nav-label { color: rgba(255,255,255,0.8) !important; }

    /* BUTTONS */
    .btn-primary { background: var(--navy); color: white; border: none; border-radius: var(--radius); padding: 14px 24px; font-size: 15px; font-weight: 500; font-family: inherit; cursor: pointer; width: 100%; transition: transform 0.15s, opacity 0.15s; }
    .btn-primary:active { transform: scale(0.97); opacity: 0.9; }
    .btn-outline { background: transparent; color: var(--navy); border: 1.5px solid var(--navy); border-radius: var(--radius); padding: 13px 24px; font-size: 15px; font-weight: 500; font-family: inherit; cursor: pointer; width: 100%; transition: all 0.15s; }
    .btn-green { background: var(--green); color: white; border: none; border-radius: var(--radius); padding: 14px 24px; font-size: 15px; font-weight: 500; font-family: inherit; cursor: pointer; width: 100%; transition: transform 0.15s; }
    .btn-green:active { transform: scale(0.97); }

    /* FORM */
    .input-field { width: 100%; padding: 14px 16px; border: 1.5px solid var(--border); border-radius: var(--radius-sm); font-size: 15px; font-family: inherit; color: var(--text); background: var(--surface); outline: none; transition: border-color 0.2s; }
    .input-field:focus { border-color: var(--green); }
    .input-label { font-size: 12px; font-weight: 500; color: var(--text-muted); margin-bottom: 6px; text-transform: uppercase; letter-spacing: 0.5px; }

    /* CARDS */
    .product-card { background: var(--surface); border-radius: var(--radius); overflow: hidden; box-shadow: var(--shadow); cursor: pointer; transition: transform 0.2s; }
    .product-card:active { transform: scale(0.97); }

    /* CHIPS */
    .chip { display: inline-flex; align-items: center; padding: 7px 14px; border-radius: 50px; border: 1.5px solid var(--border); font-size: 13px; font-weight: 400; cursor: pointer; transition: all 0.2s; white-space: nowrap; color: var(--text); background: var(--surface); }
    .chip.selected { background: var(--navy); color: white; border-color: var(--navy); }
    .chip-green { background: var(--green-light); color: var(--green-dark); border-color: var(--green-light); }

    /* HEADER */
    .screen-header { background: var(--surface); padding: 12px 20px; border-bottom: 1px solid var(--border); flex-shrink: 0; }
    .back-btn { display: flex; align-items: center; gap: 8px; color: var(--text); font-size: 15px; cursor: pointer; background: none; border: none; font-family: inherit; padding: 0; }

    /* MISC */
    .avatar { border-radius: 50%; object-fit: cover; }
    .avatar-placeholder { border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 600; color: white; background: var(--green); flex-shrink: 0; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(8px); } to { opacity: 1; transform: translateY(0); } }
    @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
    .fade-in { animation: fadeIn 0.4s ease forwards; }
    .slide-up { animation: slideUp 0.35s ease forwards; }
    .progress-bar { height: 4px; background: var(--border); border-radius: 2px; overflow: hidden; }
    .progress-fill { height: 100%; background: var(--green); border-radius: 2px; transition: width 0.4s ease; }
    .stars { color: #FFB800; font-size: 14px; }
    .badge { background: var(--green); color: white; border-radius: 50px; font-size: 11px; font-weight: 600; padding: 2px 8px; }
    .badge-navy { background: var(--navy); }
    .verified-badge { background: var(--green-light); color: var(--green-dark); border-radius: 50px; font-size: 11px; font-weight: 500; padding: 3px 10px; display: inline-flex; align-items: center; gap: 4px; }
    .notif-dot { width: 8px; height: 8px; background: #FF4757; border-radius: 50%; position: absolute; top: 6px; right: 8px; }
    .search-bar { display: flex; align-items: center; background: var(--bg); border: 1.5px solid var(--border); border-radius: 50px; padding: 10px 16px; gap: 10px; cursor: text; }
    .search-bar i { color: var(--text-muted); font-size: 18px; }
    .search-bar span { color: var(--text-muted); font-size: 14px; }
    .like-btn { width: 32px; height: 32px; border-radius: 50%; background: rgba(255,255,255,0.9); border: none; display: flex; align-items: center; justify-content: center; cursor: pointer; transition: transform 0.2s; }
    .like-btn:active { transform: scale(0.85); }
    .like-btn i { font-size: 16px; color: var(--text-muted); }
    .like-btn.liked i { color: #FF4757; }
    .cat-scroll { display: flex; gap: 8px; overflow-x: auto; padding: 12px 20px; scrollbar-width: none; }
    .cat-scroll::-webkit-scrollbar { display: none; }
    .product-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; padding: 0 16px 16px; }
    .section-title { font-size: 18px; font-weight: 600; color: var(--text); padding: 16px 20px 8px; }
    .section-row { display: flex; justify-content: space-between; align-items: center; padding: 16px 20px 8px; }
    .see-all { font-size: 13px; color: var(--green); font-weight: 500; cursor: pointer; }
    .outfit-card { background: var(--surface); border-radius: var(--radius); padding: 16px; box-shadow: var(--shadow); cursor: pointer; transition: transform 0.2s; min-width: 180px; }
    .outfit-card:active { transform: scale(0.97); }
    .bubble-sent { background: var(--navy); color: white; border-radius: 18px 18px 4px 18px; padding: 10px 14px; max-width: 75%; font-size: 14px; align-self: flex-end; }
    .bubble-recv { background: var(--surface); color: var(--text); border-radius: 18px 18px 18px 4px; padding: 10px 14px; max-width: 75%; font-size: 14px; border: 1px solid var(--border); }
    .divider { height: 1px; background: var(--border); margin: 0 20px; }
    .photo-upload-zone { border: 2px dashed var(--border); border-radius: var(--radius); padding: 28px; display: flex; flex-direction: column; align-items: center; gap: 8px; cursor: pointer; transition: all 0.2s; position: relative; overflow: hidden; }
    .photo-upload-zone:hover { border-color: var(--green); background: var(--green-light); }
    .photo-upload-zone input[type=file] { position: absolute; inset: 0; opacity: 0; cursor: pointer; width: 100%; height: 100%; }
    .tab-bar { display: flex; border-bottom: 1px solid var(--border); background: var(--surface); flex-shrink: 0; }
    .tab-item { flex: 1; padding: 12px 0; text-align: center; font-size: 13px; font-weight: 500; color: var(--text-muted); cursor: pointer; border-bottom: 2px solid transparent; transition: all 0.2s; }
    .tab-item.active { color: var(--green); border-bottom-color: var(--green); }
    .photo-preview-grid { display: flex; flex-wrap: wrap; gap: 8px; }
    .photo-thumb { width: 80px; height: 80px; border-radius: 10px; object-fit: cover; border: 2px solid var(--green); }
    .profile-avatar-wrap { position: relative; display: inline-block; cursor: pointer; }
    .profile-avatar-wrap input[type=file] { position: absolute; inset: 0; opacity: 0; cursor: pointer; border-radius: 50%; }
    .edit-avatar-badge { position: absolute; bottom: 0; right: 0; background: var(--green); border-radius: 50%; width: 28px; height: 28px; display: flex; align-items: center; justify-content: center; border: 2px solid white; pointer-events: none; }
  </style>
</head>
<body>
<div id="app-wrap">
<div id="app">

  <!-- ======================== WELCOME ======================== -->
  <div class="screen active" id="screen-welcome">
    <div style="flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:40px 32px;background:linear-gradient(160deg,#0D2340 0%,#1a3a5c 50%,#0D2340 100%);">
      <div class="fade-in" style="display:flex;flex-direction:column;align-items:center;margin-bottom:48px;">
        <svg width="90" height="110" viewBox="0 0 100 120" fill="none" xmlns="http://www.w3.org/2000/svg" style="margin-bottom:20px;">
          <path d="M50 6 C50 6 22 6 16 26 L16 90 C16 98 22 104 30 104 L70 104 C78 104 84 98 84 90 L84 26 C78 6 50 6 50 6Z" stroke="white" stroke-width="3" fill="none"/>
          <circle cx="50" cy="6" r="5" stroke="white" stroke-width="3" fill="none"/>
          <line x1="50" y1="1" x2="50" y2="11" stroke="white" stroke-width="3"/>
          <path d="M35 52 C35 52 28 48 28 41 C28 36 32 32 37 32 C40 32 43 34 44 37 L50 45 L56 37 C57 34 60 32 63 32 C68 32 72 36 72 41 C72 48 65 52 65 52 L50 68 Z" fill="#6BB58C"/>
        </svg>
        <div style="font-family:'Playfair Display',serif;font-size:44px;font-weight:500;color:white;letter-spacing:4px;">LOOK</div>
        <div style="font-size:13px;color:rgba(255,255,255,0.55);letter-spacing:2.5px;margin-top:6px;font-weight:300;">COMPRA. VENDE. REUTILIZA.</div>
      </div>
      <div style="width:100%;display:flex;flex-direction:column;gap:14px;" class="slide-up">
        <button class="btn-primary" style="background:white;color:var(--navy);font-weight:600;" onclick="go('screen-login')">Iniciar sesión</button>
        <button class="btn-outline" style="border-color:rgba(255,255,255,0.4);color:white;" onclick="go('screen-reg1')">Crear cuenta</button>
      </div>
      <div style="margin-top:36px;font-size:11px;color:rgba(255,255,255,0.3);letter-spacing:1px;">Segunda mano, primera calidad.</div>
    </div>
  </div>

  <!-- ======================== LOGIN ======================== -->
  <div class="screen" id="screen-login">
    <div style="background:var(--navy);padding:52px 28px 32px;flex-shrink:0;">
      <button class="back-btn" style="color:rgba(255,255,255,0.6);margin-bottom:24px;" onclick="go('screen-welcome')"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="font-family:'Playfair Display',serif;font-size:28px;color:white;">Bienvenida de<br/>vuelta ✨</div>
      <div style="font-size:14px;color:rgba(255,255,255,0.45);margin-top:8px;">Tu clóset te espera</div>
    </div>
    <div class="scroll-area" style="padding:28px 24px;display:flex;flex-direction:column;gap:16px;">
      <div><div class="input-label">Correo electrónico</div><input class="input-field" id="login-email" type="email" placeholder="hola@look.cr" /></div>
      <div><div class="input-label">Contraseña</div><input class="input-field" id="login-pass" type="password" placeholder="••••••••" /></div>
      <div style="text-align:right;margin-top:-8px;"><span style="font-size:13px;color:var(--green);cursor:pointer;font-weight:500;">¿Olvidaste tu contraseña?</span></div>
      <button class="btn-primary" onclick="doLogin()" style="margin-top:4px;">Ingresar</button>
      <p id="login-error" style="color:#FF4757;font-size:13px;text-align:center;display:none;">Credenciales incorrectas. Intenta de nuevo.</p>
      <div style="text-align:center;color:var(--text-muted);font-size:14px;">¿No tienes cuenta? <span style="color:var(--green);font-weight:500;cursor:pointer;" onclick="go('screen-reg1')">Crear una</span></div>
    </div>
  </div>

  <!-- ======================== REGISTER STEP 1 ======================== -->
  <div class="screen" id="screen-reg1">
    <div style="background:var(--surface);padding:52px 24px 16px;flex-shrink:0;">
      <button class="back-btn" onclick="go('screen-welcome')" style="margin-bottom:16px;"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;">
        <div style="font-size:11px;color:var(--text-muted);font-weight:500;">PASO 1 DE 4</div>
        <div style="font-size:11px;color:var(--green);font-weight:500;">Datos básicos</div>
      </div>
      <div class="progress-bar"><div class="progress-fill" style="width:25%"></div></div>
      <div style="font-size:21px;font-weight:600;color:var(--text);margin-top:14px;">Cuéntanos sobre ti</div>
    </div>
    <div class="scroll-area" style="padding:20px 24px;display:flex;flex-direction:column;gap:14px;">
      <div><div class="input-label">Nombre completo</div><input class="input-field" id="reg-name" type="text" placeholder="Tu nombre completo" /></div>
      <div><div class="input-label">Correo electrónico</div><input class="input-field" id="reg-email" type="email" placeholder="hola@look.cr" /></div>
      <div><div class="input-label">Contraseña</div><input class="input-field" id="reg-pass" type="password" placeholder="Mínimo 8 caracteres" /></div>
      <div>
        <div class="input-label" style="margin-bottom:10px;">Género</div>
        <div style="display:flex;gap:8px;">
          <div class="chip selected" data-group="gender" onclick="selectChip(this,'gender')">Mujer</div>
          <div class="chip" data-group="gender" onclick="selectChip(this,'gender')">Hombre</div>
          <div class="chip" data-group="gender" onclick="selectChip(this,'gender')">Otro</div>
        </div>
      </div>
      <p id="reg1-error" style="color:#FF4757;font-size:13px;display:none;">Por favor completa todos los campos.</p>
      <button class="btn-primary" onclick="goReg2()">Continuar <i class="ti ti-arrow-right"></i></button>
    </div>
  </div>

  <!-- ======================== REGISTER STEP 2 ======================== -->
  <div class="screen" id="screen-reg2">
    <div style="background:var(--surface);padding:52px 24px 16px;flex-shrink:0;">
      <button class="back-btn" onclick="go('screen-reg1')" style="margin-bottom:16px;"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;">
        <div style="font-size:11px;color:var(--text-muted);font-weight:500;">PASO 2 DE 4</div>
        <div style="font-size:11px;color:var(--green);font-weight:500;">Tallas y estilo</div>
      </div>
      <div class="progress-bar"><div class="progress-fill" style="width:50%"></div></div>
      <div style="font-size:21px;font-weight:600;color:var(--text);margin-top:14px;">Tu perfil de moda</div>
    </div>
    <div class="scroll-area" style="padding:20px 24px;display:flex;flex-direction:column;gap:14px;">
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
        <div>
          <div class="input-label">Talla superior</div>
          <select class="input-field" id="reg-size-top" style="appearance:none;cursor:pointer;">
            <option>XS</option><option>S</option><option selected>M</option><option>L</option><option>XL</option><option>XXL</option>
          </select>
        </div>
        <div>
          <div class="input-label">Talla inferior</div>
          <select class="input-field" id="reg-size-bot" style="appearance:none;cursor:pointer;">
            <option>36</option><option>38</option><option selected>40</option><option>42</option><option>44</option>
          </select>
        </div>
      </div>
      <div>
        <div class="input-label">Talla de zapatos</div>
        <select class="input-field" id="reg-size-shoes" style="appearance:none;cursor:pointer;">
          <option>34</option><option>35</option><option>36</option><option selected>37</option><option>38</option><option>39</option><option>40</option><option>41</option>
        </select>
      </div>
      <div>
        <div class="input-label" style="margin-bottom:10px;">Estilos favoritos (elige varios)</div>
        <div style="display:flex;flex-wrap:wrap;gap:8px;" id="styles-wrap">
          <div class="chip selected" data-group="styles" onclick="toggleChip(this)">Casual</div>
          <div class="chip" data-group="styles" onclick="toggleChip(this)">Vintage</div>
          <div class="chip" data-group="styles" onclick="toggleChip(this)">Streetwear</div>
          <div class="chip" data-group="styles" onclick="toggleChip(this)">Elegante</div>
          <div class="chip" data-group="styles" onclick="toggleChip(this)">Deportivo</div>
          <div class="chip selected" data-group="styles" onclick="toggleChip(this)">Minimalista</div>
          <div class="chip" data-group="styles" onclick="toggleChip(this)">Y2K</div>
          <div class="chip" data-group="styles" onclick="toggleChip(this)">Boho</div>
        </div>
      </div>
      <div>
        <div class="input-label" style="margin-bottom:10px;">Categorías de interés</div>
        <div style="display:flex;flex-wrap:wrap;gap:8px;" id="cats-wrap">
          <div class="chip selected" onclick="toggleChip(this)">👚 Blusas</div>
          <div class="chip" onclick="toggleChip(this)">👖 Jeans</div>
          <div class="chip selected" onclick="toggleChip(this)">👟 Zapatos</div>
          <div class="chip" onclick="toggleChip(this)">👗 Vestidos</div>
          <div class="chip" onclick="toggleChip(this)">🧥 Jackets</div>
          <div class="chip" onclick="toggleChip(this)">👜 Bolsos</div>
          <div class="chip" onclick="toggleChip(this)">💍 Accesorios</div>
        </div>
      </div>
      <button class="btn-primary" onclick="goReg3()">Continuar <i class="ti ti-arrow-right"></i></button>
    </div>
  </div>

  <!-- ======================== REGISTER STEP 3 ======================== -->
  <div class="screen" id="screen-reg3">
    <div style="background:var(--surface);padding:52px 24px 16px;flex-shrink:0;">
      <button class="back-btn" onclick="go('screen-reg2')" style="margin-bottom:16px;"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;">
        <div style="font-size:11px;color:var(--text-muted);font-weight:500;">PASO 3 DE 4</div>
        <div style="font-size:11px;color:var(--green);font-weight:500;">Foto de perfil</div>
      </div>
      <div class="progress-bar"><div class="progress-fill" style="width:75%"></div></div>
      <div style="font-size:21px;font-weight:600;color:var(--text);margin-top:14px;">Agrega una foto</div>
      <div style="font-size:13px;color:var(--text-muted);margin-top:4px;">Opcional, pero genera más confianza 😊</div>
    </div>
    <div class="scroll-area" style="padding:24px;display:flex;flex-direction:column;align-items:center;gap:24px;">
      <!-- Avatar preview -->
      <div style="position:relative;display:inline-block;">
        <div id="reg-avatar-preview" class="avatar-placeholder" style="width:120px;height:120px;font-size:40px;font-weight:300;">📷</div>
        <img id="reg-avatar-img" src="" alt="" style="width:120px;height:120px;border-radius:50%;object-fit:cover;display:none;border:3px solid var(--green);" />
        <div style="position:absolute;bottom:4px;right:4px;background:var(--green);border-radius:50%;width:32px;height:32px;display:flex;align-items:center;justify-content:center;border:2px solid white;">
          <i class="ti ti-camera" style="color:white;font-size:16px;"></i>
        </div>
        <input type="file" accept="image/*" onchange="previewAvatar(this)" style="position:absolute;inset:0;opacity:0;cursor:pointer;border-radius:50%;width:100%;height:100%;" />
      </div>
      <p style="font-size:13px;color:var(--text-muted);text-align:center;">Toca la foto para seleccionar una imagen de tu galería o cámara</p>
      <div style="width:100%;display:flex;flex-direction:column;gap:10px;">
        <button class="btn-primary" onclick="goReg4()">Continuar <i class="ti ti-arrow-right"></i></button>
        <button class="btn-outline" onclick="goReg4()">Omitir por ahora</button>
      </div>
    </div>
  </div>

  <!-- ======================== REGISTER STEP 4 ======================== -->
  <div class="screen" id="screen-reg4">
    <div style="background:var(--surface);padding:52px 24px 16px;flex-shrink:0;">
      <button class="back-btn" onclick="go('screen-reg3')" style="margin-bottom:16px;"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;">
        <div style="font-size:11px;color:var(--text-muted);font-weight:500;">PASO 4 DE 4</div>
        <div style="font-size:11px;color:var(--green);font-weight:500;">¡Casi listo!</div>
      </div>
      <div class="progress-bar"><div class="progress-fill" style="width:100%"></div></div>
      <div style="font-size:21px;font-weight:600;color:var(--text);margin-top:14px;">¿Qué quieres hacer?</div>
    </div>
    <div class="scroll-area" style="padding:20px 24px;display:flex;flex-direction:column;gap:12px;">
      <div onclick="selectIntent(this)" style="background:var(--surface);border:2px solid var(--green);border-radius:var(--radius);padding:20px;cursor:pointer;transition:all 0.2s;">
        <div style="font-size:28px;margin-bottom:6px;">🛍️</div>
        <div style="font-weight:600;font-size:15px;">Comprar</div>
        <div style="font-size:13px;color:var(--text-muted);margin-top:4px;">Encuentra prendas únicas a precios increíbles</div>
      </div>
      <div onclick="selectIntent(this)" style="background:var(--surface);border:2px solid var(--border);border-radius:var(--radius);padding:20px;cursor:pointer;transition:all 0.2s;">
        <div style="font-size:28px;margin-bottom:6px;">💰</div>
        <div style="font-weight:600;font-size:15px;">Vender</div>
        <div style="font-size:13px;color:var(--text-muted);margin-top:4px;">Dale nueva vida a tu ropa y gana dinero</div>
      </div>
      <div onclick="selectIntent(this)" style="background:var(--surface);border:2px solid var(--border);border-radius:var(--radius);padding:20px;cursor:pointer;transition:all 0.2s;">
        <div style="font-size:28px;margin-bottom:6px;">✨</div>
        <div style="font-weight:600;font-size:15px;">Comprar y vender</div>
        <div style="font-size:13px;color:var(--text-muted);margin-top:4px;">Renueva tu clóset: vende lo viejo, compra lo nuevo</div>
      </div>
      <button class="btn-primary" onclick="finishRegister()" style="margin-top:4px;">Crear mi cuenta 🎉</button>
    </div>
  </div>

  <!-- ======================== REGISTER DONE ======================== -->
  <div class="screen" id="screen-reg-done">
    <div style="flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:40px 32px;text-align:center;background:white;">
      <div id="done-avatar-wrap" style="margin-bottom:20px;"></div>
      <div style="width:64px;height:64px;background:var(--green-light);border-radius:50%;display:flex;align-items:center;justify-content:center;margin-bottom:20px;" id="done-check-icon">
        <i class="ti ti-check" style="font-size:32px;color:var(--green);"></i>
      </div>
      <div style="font-family:'Playfair Display',serif;font-size:26px;font-weight:400;color:var(--navy);margin-bottom:8px;">¡Tu perfil está listo!</div>
      <div id="done-welcome-name" style="font-size:15px;color:var(--text-muted);margin-bottom:28px;">Bienvenida a LOOK 🌿</div>
      <div style="display:flex;flex-wrap:wrap;gap:10px;justify-content:center;margin-bottom:36px;" id="done-chips"></div>
      <button class="btn-primary" onclick="go('screen-home');renderHome();">Explorar LOOK 🎉</button>
    </div>
  </div>

  <!-- ======================== HOME ======================== -->
  <div class="screen" id="screen-home">
    <div class="status-bar"><span class="status-time" id="status-time-home">9:41</span><div class="status-icons"><i class="ti ti-wifi"></i><i class="ti ti-battery-2"></i></div></div>
    <div style="background:var(--surface);padding:12px 20px;flex-shrink:0;">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;">
        <div>
          <div style="font-size:13px;color:var(--text-muted);" id="home-greeting">Hola 👋</div>
          <div style="font-family:'Playfair Display',serif;font-size:22px;color:var(--navy);">¿Qué llevas hoy?</div>
        </div>
        <div style="position:relative;cursor:pointer;" onclick="go('screen-notifications')">
          <div style="width:40px;height:40px;background:var(--bg);border-radius:50%;display:flex;align-items:center;justify-content:center;">
            <i class="ti ti-bell" style="font-size:20px;color:var(--text);"></i>
          </div>
          <div class="notif-dot"></div>
        </div>
      </div>
      <div class="search-bar" onclick="go('screen-explore')">
        <i class="ti ti-search"></i><span>Buscar prendas, marcas...</span>
      </div>
    </div>
    <div class="cat-scroll" style="background:var(--surface);padding-bottom:14px;">
      <div class="chip selected" onclick="filterCat(this,'Todo')">Todo</div>
      <div class="chip" onclick="filterCat(this,'Blusas')">Blusas</div>
      <div class="chip" onclick="filterCat(this,'Jeans')">Jeans</div>
      <div class="chip" onclick="filterCat(this,'Zapatos')">Zapatos</div>
      <div class="chip" onclick="filterCat(this,'Vintage')">Vintage</div>
      <div class="chip" onclick="filterCat(this,'Streetwear')">Streetwear</div>
      <div class="chip" onclick="filterCat(this,'Formal')">Formal</div>
    </div>
    <div class="scroll-area">
      <div style="margin:0 16px 4px;background:linear-gradient(135deg,var(--navy),#1a3a5c);border-radius:var(--radius);padding:20px;color:white;position:relative;overflow:hidden;">
        <div style="position:absolute;right:-10px;top:-10px;width:80px;height:80px;background:rgba(107,181,140,0.3);border-radius:50%;"></div>
        <div style="font-size:11px;color:rgba(255,255,255,0.55);margin-bottom:4px;letter-spacing:1px;">RECOMENDACIONES ✨</div>
        <div style="font-size:17px;font-weight:600;margin-bottom:12px;">Outfits basados<br/>en tu estilo</div>
        <button onclick="go('screen-ai')" style="background:var(--green);color:white;border:none;border-radius:50px;padding:8px 18px;font-size:13px;font-weight:500;font-family:inherit;cursor:pointer;">Ver recomendaciones →</button>
      </div>
      <div class="section-row"><div class="section-title" style="padding:0;">Recién llegados</div><span class="see-all" onclick="go('screen-explore')">Ver todo</span></div>
      <div class="product-grid" id="home-product-grid"></div>
      <div style="height:20px;"></div>
    </div>
    <div class="bottom-nav">
      <div class="nav-item active" onclick="switchTab('home')"><i class="ti ti-home nav-icon"></i><span class="nav-label">Inicio</span></div>
      <div class="nav-item" onclick="switchTab('explore')"><i class="ti ti-compass nav-icon"></i><span class="nav-label">Explorar</span></div>
      <div class="nav-item nav-ai" onclick="go('screen-ai')"><i class="ti ti-sparkles nav-icon"></i><span class="nav-label">IA</span></div>
      <div class="nav-item" onclick="switchTab('favorites')"><i class="ti ti-heart nav-icon"></i><span class="nav-label">Favoritos</span></div>
      <div class="nav-item" onclick="switchTab('profile')"><i class="ti ti-user nav-icon"></i><span class="nav-label">Perfil</span></div>
    </div>
  </div>

  <!-- ======================== PRODUCT DETAIL ======================== -->
  <div class="screen" id="screen-product">
    <div style="position:relative;height:300px;flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:90px;" id="product-hero-bg">
      <span id="product-hero-emoji">👗</span>
      <button class="back-btn" id="product-back-btn" style="position:absolute;top:52px;left:16px;background:rgba(255,255,255,0.9);border-radius:50px;padding:8px 14px;box-shadow:var(--shadow);" onclick="go('screen-home')"><i class="ti ti-arrow-left"></i></button>
      <button style="position:absolute;top:52px;right:16px;background:rgba(255,255,255,0.9);border-radius:50%;width:40px;height:40px;border:none;display:flex;align-items:center;justify-content:center;box-shadow:var(--shadow);cursor:pointer;" onclick="toggleFav(this,currentProduct)"><i class="ti ti-heart" style="font-size:18px;color:var(--text-muted);" id="fav-heart-icon"></i></button>
    </div>
    <div class="scroll-area" style="background:var(--surface);">
      <div style="padding:20px;">
        <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:8px;">
          <div><div style="font-size:19px;font-weight:600;" id="pd-name">Vestido midi</div><div style="font-size:13px;color:var(--text-muted);margin-top:2px;" id="pd-brand">Zara · Buen estado</div></div>
          <div style="font-size:22px;font-weight:700;color:var(--navy);" id="pd-price">₡12,000</div>
        </div>
        <div style="display:flex;gap:8px;margin-bottom:14px;">
          <div class="chip chip-green" style="font-size:12px;padding:5px 12px;" id="pd-size">Talla M</div>
          <div class="chip chip-green" style="font-size:12px;padding:5px 12px;" id="pd-cat">Vestido</div>
        </div>
        <div style="font-size:14px;color:var(--text-muted);line-height:1.6;margin-bottom:14px;" id="pd-desc">Descripción del producto.</div>
        <div style="background:var(--bg);border-radius:var(--radius-sm);padding:14px;margin-bottom:14px;">
          <div style="display:flex;justify-content:space-between;font-size:13px;margin-bottom:6px;"><span style="color:var(--text-muted);">Envío estimado</span><span id="pd-ship">₡2,000</span></div>
          <div style="display:flex;justify-content:space-between;font-size:13px;"><span style="color:var(--text-muted);">Entrega estimada</span><span>3-5 días hábiles</span></div>
        </div>
        <div style="display:flex;align-items:center;gap:12px;padding:14px;background:var(--bg);border-radius:var(--radius-sm);margin-bottom:14px;cursor:pointer;" onclick="go('screen-seller')">
          <div class="avatar-placeholder" style="width:44px;height:44px;font-size:16px;">M</div>
          <div style="flex:1;"><div style="font-weight:500;font-size:14px;">@mia.shop</div><div style="display:flex;align-items:center;gap:4px;margin-top:2px;"><span class="stars">★★★★★</span><span style="font-size:12px;color:var(--text-muted);">4.9 (127 ventas)</span></div></div>
          <div class="verified-badge"><i class="ti ti-check" style="font-size:11px;"></i>Verificada</div>
        </div>
        <div style="display:flex;gap:10px;">
          <button class="btn-outline" onclick="go('screen-chat')" style="flex:1;"><i class="ti ti-message-circle"></i> Mensaje</button>
          <button class="btn-primary" onclick="go('screen-payment')" style="flex:2;">Comprar ahora</button>
        </div>
      </div>
      <div class="divider"></div>
      <div class="section-title">También te puede gustar</div>
      <div style="display:flex;gap:12px;padding:0 16px 20px;overflow-x:auto;scrollbar-width:none;">
        <div class="product-card" style="min-width:120px;" onclick="openProduct(products[1])"><div style="height:110px;background:linear-gradient(135deg,#FFE8E8,#F5C0C0);display:flex;align-items:center;justify-content:center;font-size:36px;">👚</div><div style="padding:8px;"><div style="font-size:11px;font-weight:500;">Crop top</div><div style="font-size:12px;font-weight:600;color:var(--navy);">₡6,500</div></div></div>
        <div class="product-card" style="min-width:120px;" onclick="openProduct(products[3])"><div style="height:110px;background:linear-gradient(135deg,#E8E0F0,#C8B8E8);display:flex;align-items:center;justify-content:center;font-size:36px;">👜</div><div style="padding:8px;"><div style="font-size:11px;font-weight:500;">Bolso mini</div><div style="font-size:12px;font-weight:600;color:var(--navy);">₡22,000</div></div></div>
        <div class="product-card" style="min-width:120px;" onclick="openProduct(products[2])"><div style="height:110px;background:linear-gradient(135deg,#D4E8D4,#A8C8A8);display:flex;align-items:center;justify-content:center;font-size:36px;">🧥</div><div style="padding:8px;"><div style="font-size:11px;font-weight:500;">Blazer vintage</div><div style="font-size:12px;font-weight:600;color:var(--navy);">₡18,500</div></div></div>
      </div>
    </div>
  </div>

  <!-- ======================== SELLER ======================== -->
  <div class="screen" id="screen-seller">
    <div style="background:var(--navy);padding:52px 20px 24px;flex-shrink:0;">
      <button class="back-btn" style="color:rgba(255,255,255,0.6);margin-bottom:18px;" onclick="go('screen-product')"><i class="ti ti-arrow-left"></i></button>
      <div style="display:flex;align-items:center;gap:14px;">
        <div class="avatar-placeholder" style="width:62px;height:62px;font-size:24px;background:var(--green);">M</div>
        <div>
          <div style="color:white;font-size:17px;font-weight:600;">Mía González</div>
          <div style="color:rgba(255,255,255,0.5);font-size:13px;">@mia.shop · San José, CR</div>
          <div style="margin-top:6px;"><div class="verified-badge">✓ Vendedora verificada</div></div>
        </div>
      </div>
      <div style="display:flex;gap:10px;margin-top:18px;">
        <div style="flex:1;background:rgba(255,255,255,0.1);border-radius:var(--radius-sm);padding:12px;text-align:center;"><div style="font-size:20px;font-weight:700;color:white;">127</div><div style="font-size:11px;color:rgba(255,255,255,0.5);">Ventas</div></div>
        <div style="flex:1;background:rgba(255,255,255,0.1);border-radius:var(--radius-sm);padding:12px;text-align:center;"><div style="font-size:20px;font-weight:700;color:white;">4.9</div><div style="font-size:11px;color:rgba(255,255,255,0.5);">Calificación</div></div>
        <div style="flex:1;background:rgba(255,255,255,0.1);border-radius:var(--radius-sm);padding:12px;text-align:center;"><div style="font-size:20px;font-weight:700;color:white;">1.2k</div><div style="font-size:11px;color:rgba(255,255,255,0.5);">Seguidores</div></div>
      </div>
    </div>
    <div style="padding:12px 16px;background:var(--surface);display:flex;gap:10px;flex-shrink:0;">
      <button class="btn-outline" style="flex:1;padding:10px;" onclick="go('screen-chat')"><i class="ti ti-message-circle"></i> Mensaje</button>
      <button class="btn-primary" style="flex:1;padding:10px;">Seguir</button>
    </div>
    <div class="scroll-area">
      <div style="padding:14px 16px;font-size:13px;color:var(--text-muted);line-height:1.6;background:var(--surface);">♻️ Apasionada por la moda sustentable. Vendo prendas de calidad. Envíos en 24h. 🌿</div>
      <div style="display:flex;gap:16px;padding:10px 16px;background:var(--surface);border-bottom:1px solid var(--border);">
        <div style="font-size:12px;color:var(--text-muted);"><i class="ti ti-truck" style="color:var(--green);"></i> Envío: ₡2,000</div>
        <div style="font-size:12px;color:var(--text-muted);"><i class="ti ti-clock" style="color:var(--green);"></i> Responde en 2h</div>
      </div>
      <div class="section-title" style="padding:14px 16px 8px;">Su clóset</div>
      <div class="product-grid">
        <div class="product-card" onclick="openProduct(products[0])"><div style="height:140px;background:linear-gradient(135deg,#E8F0F8,#C5D5E8);display:flex;align-items:center;justify-content:center;font-size:48px;">👗</div><div style="padding:8px;"><div style="font-size:12px;font-weight:500;">Vestido midi</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡12,000</div></div></div>
        <div class="product-card" onclick="openProduct(products[1])"><div style="height:140px;background:linear-gradient(135deg,#FFE8E8,#F5C0C0);display:flex;align-items:center;justify-content:center;font-size:48px;">👚</div><div style="padding:8px;"><div style="font-size:12px;font-weight:500;">Blusa floral</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡7,500</div></div></div>
        <div class="product-card" onclick="openProduct(products[2])"><div style="height:140px;background:linear-gradient(135deg,#D4E8D4,#A8C8A8);display:flex;align-items:center;justify-content:center;font-size:48px;">🧥</div><div style="padding:8px;"><div style="font-size:12px;font-weight:500;">Blazer</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡18,500</div></div></div>
        <div class="product-card" onclick="openProduct(products[3])"><div style="height:140px;background:linear-gradient(135deg,#E8E0F0,#C8B8E8);display:flex;align-items:center;justify-content:center;font-size:48px;">👜</div><div style="padding:8px;"><div style="font-size:12px;font-weight:500;">Bolso mini</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡22,000</div></div></div>
      </div>
    </div>
  </div>

  <!-- ======================== EXPLORE ======================== -->
  <div class="screen" id="screen-explore">
    <div class="status-bar"><span class="status-time">9:41</span><div class="status-icons"><i class="ti ti-wifi"></i><i class="ti ti-battery-2"></i></div></div>
    <div style="background:var(--surface);padding:12px 20px 16px;flex-shrink:0;">
      <div style="font-size:22px;font-weight:600;margin-bottom:12px;">Explorar</div>
      <div style="display:flex;gap:10px;">
        <div class="search-bar" style="flex:1;"><i class="ti ti-search"></i><span>Buscar...</span></div>
        <div style="width:44px;height:44px;background:var(--navy);border-radius:var(--radius-sm);display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0;"><i class="ti ti-adjustments" style="color:white;font-size:18px;"></i></div>
      </div>
    </div>
    <div class="scroll-area">
      <div class="section-title" style="padding-bottom:8px;">Categorías</div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;padding:0 16px;">
        <div style="background:linear-gradient(135deg,#0D2340,#1a3a5c);border-radius:var(--radius);padding:20px;cursor:pointer;color:white;" onclick="go('screen-home')"><div style="font-size:28px;margin-bottom:8px;">👗</div><div style="font-weight:600;">Vestidos</div><div style="font-size:12px;opacity:0.6;">234 prendas</div></div>
        <div style="background:linear-gradient(135deg,#6BB58C,#4a8a68);border-radius:var(--radius);padding:20px;cursor:pointer;color:white;" onclick="go('screen-home')"><div style="font-size:28px;margin-bottom:8px;">👟</div><div style="font-weight:600;">Zapatos</div><div style="font-size:12px;opacity:0.6;">456 prendas</div></div>
        <div style="background:linear-gradient(135deg,#8B6BB5,#6a4a8a);border-radius:var(--radius);padding:20px;cursor:pointer;color:white;" onclick="go('screen-home')"><div style="font-size:28px;margin-bottom:8px;">🧥</div><div style="font-weight:600;">Outerwear</div><div style="font-size:12px;opacity:0.6;">189 prendas</div></div>
        <div style="background:linear-gradient(135deg,#B58B6B,#8a6b4a);border-radius:var(--radius);padding:20px;cursor:pointer;color:white;" onclick="go('screen-home')"><div style="font-size:28px;margin-bottom:8px;">👜</div><div style="font-weight:600;">Accesorios</div><div style="font-size:12px;opacity:0.6;">312 prendas</div></div>
      </div>
      <div class="section-row" style="margin-top:8px;"><div class="section-title" style="padding:0;">Tendencias 🔥</div></div>
      <div class="product-grid">
        <div class="product-card" onclick="openProduct(products[4])"><div style="height:150px;background:linear-gradient(135deg,#E8F4FF,#B8D8F8);display:flex;align-items:center;justify-content:center;font-size:48px;">👖</div><div style="padding:10px;"><div style="font-size:13px;font-weight:500;">Jeans Levi's 501</div><div style="font-size:14px;font-weight:600;color:var(--navy);">₡28,000</div></div></div>
        <div class="product-card" onclick="openProduct(products[2])"><div style="height:150px;background:linear-gradient(135deg,#D4E8D4,#A8C8A8);display:flex;align-items:center;justify-content:center;font-size:48px;">🧥</div><div style="padding:10px;"><div style="font-size:13px;font-weight:500;">Blazer vintage</div><div style="font-size:14px;font-weight:600;color:var(--navy);">₡18,500</div></div></div>
        <div class="product-card" onclick="openProduct(products[5])"><div style="height:150px;background:linear-gradient(135deg,#F5E6D3,#E8C9A0);display:flex;align-items:center;justify-content:center;font-size:48px;">👟</div><div style="padding:10px;"><div style="font-size:13px;font-weight:500;">Nike AF1</div><div style="font-size:14px;font-weight:600;color:var(--navy);">₡35,000</div></div></div>
        <div class="product-card" onclick="openProduct(products[3])"><div style="height:150px;background:linear-gradient(135deg,#FFE8F4,#F5C0D8);display:flex;align-items:center;justify-content:center;font-size:48px;">💍</div><div style="padding:10px;"><div style="font-size:13px;font-weight:500;">Set accesorios</div><div style="font-size:14px;font-weight:600;color:var(--navy);">₡9,000</div></div></div>
      </div>
      <div style="height:20px;"></div>
    </div>
    <div class="bottom-nav">
      <div class="nav-item" onclick="switchTab('home')"><i class="ti ti-home nav-icon"></i><span class="nav-label">Inicio</span></div>
      <div class="nav-item active" onclick="switchTab('explore')"><i class="ti ti-compass nav-icon"></i><span class="nav-label">Explorar</span></div>
      <div class="nav-item nav-ai" onclick="go('screen-ai')"><i class="ti ti-sparkles nav-icon"></i><span class="nav-label">IA</span></div>
      <div class="nav-item" onclick="switchTab('favorites')"><i class="ti ti-heart nav-icon"></i><span class="nav-label">Favoritos</span></div>
      <div class="nav-item" onclick="switchTab('profile')"><i class="ti ti-user nav-icon"></i><span class="nav-label">Perfil</span></div>
    </div>
  </div>

  <!-- ======================== AI ======================== -->
  <div class="screen" id="screen-ai">
    <div class="status-bar"><span class="status-time">9:41</span><div class="status-icons"><i class="ti ti-wifi"></i><i class="ti ti-battery-2"></i></div></div>
    <div style="background:linear-gradient(160deg,var(--navy),#1a3a5c);padding:16px 20px 20px;flex-shrink:0;">
      <div style="font-size:11px;color:rgba(255,255,255,0.4);letter-spacing:1px;margin-bottom:6px;">TU GUÍA</div>
      <div style="display:flex;align-items:center;gap:12px;">
        <div style="width:44px;height:44px;background:var(--green);border-radius:14px;display:flex;align-items:center;justify-content:center;font-size:22px;">✨</div>
        <div><div style="font-family:'Playfair Display',serif;font-size:22px;color:white;">LOOK IA</div><div style="font-size:12px;color:rgba(255,255,255,0.4);">Estilista personal</div></div>
      </div>
      <div style="background:rgba(255,255,255,0.1);border-radius:var(--radius-sm);padding:14px;margin-top:14px;">
        <div style="font-size:13px;color:rgba(255,255,255,0.8);line-height:1.6;" id="ai-greeting">¡Hola! Hoy te tengo combinaciones perfectas basadas en tu estilo 🌿</div>
      </div>
    </div>
    <div class="scroll-area">
      <div class="section-title" style="padding-bottom:8px;">Looks del día 🌟</div>
      <div style="display:flex;overflow-x:auto;padding:0 16px 16px;gap:12px;scrollbar-width:none;">
        <div class="outfit-card" onclick="openProduct(products[0])"><div style="font-size:36px;text-align:center;margin-bottom:10px;">👗✨</div><div style="font-weight:600;font-size:14px;">Look casual</div><div style="font-size:12px;color:var(--text-muted);margin-bottom:10px;">Vestido midi + sneakers</div><div style="display:flex;gap:4px;"><div class="badge" style="font-size:10px;">3 prendas</div><div class="badge badge-navy" style="font-size:10px;">₡28k</div></div></div>
        <div class="outfit-card" onclick="openProduct(products[2])"><div style="font-size:36px;text-align:center;margin-bottom:10px;">🎸🖤</div><div style="font-weight:600;font-size:14px;">Outfit concierto</div><div style="font-size:12px;color:var(--text-muted);margin-bottom:10px;">Blazer + jeans + boots</div><div style="display:flex;gap:4px;"><div class="badge" style="font-size:10px;">4 prendas</div><div class="badge badge-navy" style="font-size:10px;">₡56k</div></div></div>
        <div class="outfit-card" onclick="openProduct(products[1])"><div style="font-size:36px;text-align:center;margin-bottom:10px;">🌿💚</div><div style="font-weight:600;font-size:14px;">Minimal clean</div><div style="font-size:12px;color:var(--text-muted);margin-bottom:10px;">Blusa + pantalón</div><div style="display:flex;gap:4px;"><div class="badge" style="font-size:10px;">3 prendas</div><div class="badge badge-navy" style="font-size:10px;">₡35k</div></div></div>
        <div class="outfit-card" onclick="openProduct(products[4])"><div style="font-size:36px;text-align:center;margin-bottom:10px;">📼🩵</div><div style="font-weight:600;font-size:14px;">Vintage Y2K</div><div style="font-size:12px;color:var(--text-muted);margin-bottom:10px;">Crop + low-rise</div><div style="display:flex;gap:4px;"><div class="badge" style="font-size:10px;">4 prendas</div><div class="badge badge-navy" style="font-size:10px;">₡42k</div></div></div>
      </div>
      <div class="divider"></div>
      <div class="section-title" style="padding-bottom:8px;">Prendas para ti 💫</div>
      <div class="product-grid">
        <div class="product-card" onclick="openProduct(products[0])"><div style="height:140px;background:linear-gradient(135deg,#E8F0F8,#C5D5E8);display:flex;align-items:center;justify-content:center;font-size:46px;">👗</div><div style="padding:10px;"><div style="font-size:11px;color:var(--green);font-weight:500;">98% match ✓</div><div style="font-size:12px;font-weight:500;">Vestido midi</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡12,000</div></div></div>
        <div class="product-card" onclick="openProduct(products[2])"><div style="height:140px;background:linear-gradient(135deg,#D4E8D4,#A8C8A8);display:flex;align-items:center;justify-content:center;font-size:46px;">🧥</div><div style="padding:10px;"><div style="font-size:11px;color:var(--green);font-weight:500;">95% match ✓</div><div style="font-size:12px;font-weight:500;">Blazer verde</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡18,500</div></div></div>
        <div class="product-card" onclick="openProduct(products[5])"><div style="height:140px;background:linear-gradient(135deg,#F5E6D3,#E8C9A0);display:flex;align-items:center;justify-content:center;font-size:46px;">👟</div><div style="padding:10px;"><div style="font-size:11px;color:var(--green);font-weight:500;">92% match ✓</div><div style="font-size:12px;font-weight:500;">Nike AF1</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡35,000</div></div></div>
        <div class="product-card" onclick="openProduct(products[3])"><div style="height:140px;background:linear-gradient(135deg,#E8E0F0,#C8B8E8);display:flex;align-items:center;justify-content:center;font-size:46px;">👜</div><div style="padding:10px;"><div style="font-size:11px;color:var(--green);font-weight:500;">90% match ✓</div><div style="font-size:12px;font-weight:500;">Bolso mini</div><div style="font-size:13px;font-weight:600;color:var(--navy);">₡22,000</div></div></div>
      </div>
      <div style="height:20px;"></div>
    </div>
    <div class="bottom-nav">
      <div class="nav-item" onclick="switchTab('home')"><i class="ti ti-home nav-icon"></i><span class="nav-label">Inicio</span></div>
      <div class="nav-item" onclick="switchTab('explore')"><i class="ti ti-compass nav-icon"></i><span class="nav-label">Explorar</span></div>
      <div class="nav-item nav-ai" style="border:2px solid var(--green);" onclick="go('screen-ai')"><i class="ti ti-sparkles nav-icon"></i><span class="nav-label">IA</span></div>
      <div class="nav-item" onclick="switchTab('favorites')"><i class="ti ti-heart nav-icon"></i><span class="nav-label">Favoritos</span></div>
      <div class="nav-item" onclick="switchTab('profile')"><i class="ti ti-user nav-icon"></i><span class="nav-label">Perfil</span></div>
    </div>
  </div>

  <!-- ======================== FAVORITES ======================== -->
  <div class="screen" id="screen-favorites">
    <div class="status-bar"><span class="status-time">9:41</span><div class="status-icons"><i class="ti ti-wifi"></i><i class="ti ti-battery-2"></i></div></div>
    <div style="background:var(--surface);padding:12px 20px 16px;flex-shrink:0;">
      <div style="font-size:22px;font-weight:600;">Favoritos ♥</div>
      <div style="font-size:13px;color:var(--text-muted);margin-top:2px;" id="fav-count">0 prendas guardadas</div>
    </div>
    <div class="scroll-area">
      <div class="product-grid" id="fav-grid" style="padding-top:14px;"></div>
      <div id="fav-empty" style="display:none;text-align:center;padding:60px 20px;">
        <div style="font-size:48px;margin-bottom:12px;">🤍</div>
        <div style="font-weight:600;font-size:16px;margin-bottom:6px;">Nada guardado aún</div>
        <div style="font-size:13px;color:var(--text-muted);margin-bottom:20px;">Dale ♥ a las prendas que te gusten</div>
        <button class="btn-primary" onclick="switchTab('explore')">Explorar prendas</button>
      </div>
      <div style="height:20px;"></div>
    </div>
    <div class="bottom-nav">
      <div class="nav-item" onclick="switchTab('home')"><i class="ti ti-home nav-icon"></i><span class="nav-label">Inicio</span></div>
      <div class="nav-item" onclick="switchTab('explore')"><i class="ti ti-compass nav-icon"></i><span class="nav-label">Explorar</span></div>
      <div class="nav-item nav-ai" onclick="go('screen-ai')"><i class="ti ti-sparkles nav-icon"></i><span class="nav-label">IA</span></div>
      <div class="nav-item active" onclick="switchTab('favorites')"><i class="ti ti-heart nav-icon"></i><span class="nav-label">Favoritos</span></div>
      <div class="nav-item" onclick="switchTab('profile')"><i class="ti ti-user nav-icon"></i><span class="nav-label">Perfil</span></div>
    </div>
  </div>

  <!-- ======================== PROFILE ======================== -->
  <div class="screen" id="screen-profile">
    <div class="status-bar"><span class="status-time">9:41</span><div class="status-icons"><i class="ti ti-wifi"></i><i class="ti ti-battery-2"></i></div></div>
    <div style="background:var(--surface);padding:10px 20px 0;flex-shrink:0;">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px;">
        <div style="font-size:18px;font-weight:600;">Mi perfil</div>
        <i class="ti ti-settings" style="font-size:22px;color:var(--text);cursor:pointer;"></i>
      </div>
      <div style="display:flex;align-items:center;gap:14px;margin-bottom:14px;">
        <!-- Profile avatar with editable option -->
        <div class="profile-avatar-wrap">
          <div id="profile-avatar-placeholder" class="avatar-placeholder" style="width:72px;height:72px;font-size:26px;">?</div>
          <img id="profile-avatar-img" src="" style="width:72px;height:72px;border-radius:50%;object-fit:cover;display:none;border:3px solid var(--green);" />
          <div class="edit-avatar-badge"><i class="ti ti-camera" style="color:white;font-size:13px;"></i></div>
          <input type="file" accept="image/*" onchange="changeProfilePhoto(this)" />
        </div>
        <div style="flex:1;">
          <div style="font-size:17px;font-weight:600;" id="profile-display-name">Usuario</div>
          <div style="font-size:13px;color:var(--text-muted);" id="profile-handle">@usuario</div>
          <div class="stars" style="margin-top:4px;">★★★★★ <span style="font-size:12px;color:var(--text-muted);font-family:'DM Sans',sans-serif;">5.0</span></div>
        </div>
      </div>
      <div style="display:flex;gap:0;border:1px solid var(--border);border-radius:var(--radius-sm);margin-bottom:12px;overflow:hidden;">
        <div style="flex:1;text-align:center;padding:12px;border-right:1px solid var(--border);"><div style="font-size:17px;font-weight:700;color:var(--navy);" id="profile-sold-count">0</div><div style="font-size:11px;color:var(--text-muted);">Vendidas</div></div>
        <div style="flex:1;text-align:center;padding:12px;border-right:1px solid var(--border);"><div style="font-size:17px;font-weight:700;color:var(--navy);">234</div><div style="font-size:11px;color:var(--text-muted);">Seguidores</div></div>
        <div style="flex:1;text-align:center;padding:12px;"><div style="font-size:17px;font-weight:700;color:var(--navy);">89</div><div style="font-size:11px;color:var(--text-muted);">Siguiendo</div></div>
      </div>
      <div style="display:flex;flex-wrap:wrap;gap:6px;padding-bottom:12px;" id="profile-style-chips"></div>
    </div>
    <div class="tab-bar">
      <div class="tab-item active" id="ptab-closet" onclick="showProfileTab('closet')">Mi clóset</div>
      <div class="tab-item" id="ptab-sold" onclick="showProfileTab('sold')">Vendidos</div>
      <div class="tab-item" id="ptab-bought" onclick="showProfileTab('bought')">Comprados</div>
    </div>
    <div class="scroll-area">
      <div style="padding:12px 16px;">
        <button class="btn-green" onclick="go('screen-upload')" style="border-radius:50px;display:flex;align-items:center;justify-content:center;gap:8px;width:100%;"><i class="ti ti-plus"></i> Subir prenda</button>
      </div>
      <div id="ptab-content-closet">
        <div class="product-grid" id="my-closet-grid"></div>
        <div id="closet-empty" style="display:none;text-align:center;padding:40px 20px;">
          <div style="font-size:44px;margin-bottom:12px;">👗</div>
          <div style="font-weight:600;margin-bottom:6px;">Tu clóset está vacío</div>
          <div style="font-size:13px;color:var(--text-muted);">Sube tu primera prenda</div>
        </div>
      </div>
      <div id="ptab-content-sold" style="display:none;text-align:center;padding:50px 20px;">
        <div style="font-size:44px;margin-bottom:12px;">✅</div>
        <div style="font-weight:600;font-size:16px;margin-bottom:6px;" id="sold-msg">0 prendas vendidas</div>
        <div style="font-size:13px;color:var(--text-muted);">¡Cada venta suma!</div>
      </div>
      <div id="ptab-content-bought" style="display:none;text-align:center;padding:50px 20px;">
        <div style="font-size:44px;margin-bottom:12px;">📦</div>
        <div style="font-weight:600;font-size:16px;margin-bottom:6px;">3 prendas compradas</div>
        <div style="font-size:13px;color:var(--text-muted);">Tu historial aparece aquí</div>
      </div>
      <div style="height:20px;"></div>
    </div>
    <div class="bottom-nav">
      <div class="nav-item" onclick="switchTab('home')"><i class="ti ti-home nav-icon"></i><span class="nav-label">Inicio</span></div>
      <div class="nav-item" onclick="switchTab('explore')"><i class="ti ti-compass nav-icon"></i><span class="nav-label">Explorar</span></div>
      <div class="nav-item nav-ai" onclick="go('screen-ai')"><i class="ti ti-sparkles nav-icon"></i><span class="nav-label">IA</span></div>
      <div class="nav-item" onclick="switchTab('favorites')"><i class="ti ti-heart nav-icon"></i><span class="nav-label">Favoritos</span></div>
      <div class="nav-item active" onclick="switchTab('profile')"><i class="ti ti-user nav-icon"></i><span class="nav-label">Perfil</span></div>
    </div>
  </div>

  <!-- ======================== UPLOAD ======================== -->
  <div class="screen" id="screen-upload">
    <div style="background:var(--surface);padding:52px 20px 12px;flex-shrink:0;border-bottom:1px solid var(--border);">
      <button class="back-btn" onclick="go('screen-profile')" style="margin-bottom:12px;"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="font-size:22px;font-weight:600;">Subir prenda</div>
    </div>
    <div class="scroll-area" style="padding:20px;">
      <!-- Photo upload zone -->
      <div style="margin-bottom:18px;">
        <div class="input-label" style="margin-bottom:10px;">Fotos de la prenda *</div>
        <div class="photo-upload-zone" id="upload-zone">
          <input type="file" accept="image/*" multiple onchange="handleProductPhotos(this)" />
          <i class="ti ti-photo-plus" style="font-size:36px;color:var(--green);"></i>
          <div style="font-weight:500;font-size:14px;">Toca para agregar fotos</div>
          <div style="font-size:12px;color:var(--text-muted);">Hasta 8 fotos · JPG, PNG</div>
        </div>
        <div class="photo-preview-grid" id="upload-previews" style="margin-top:10px;"></div>
      </div>
      <div style="display:flex;flex-direction:column;gap:14px;">
        <div><div class="input-label">Título *</div><input class="input-field" id="up-title" placeholder="Ej: Blazer Zara talla M color verde" /></div>
        <div><div class="input-label">Descripción</div><textarea class="input-field" id="up-desc" rows="3" placeholder="Estado, uso, detalles importantes..." style="resize:none;"></textarea></div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
          <div><div class="input-label">Categoría</div><select class="input-field" id="up-cat" style="appearance:none;cursor:pointer;"><option>Blusas</option><option>Jeans</option><option>Zapatos</option><option>Vestidos</option><option>Jackets</option><option>Bolsos</option><option>Accesorios</option></select></div>
          <div><div class="input-label">Talla</div><select class="input-field" id="up-size" style="appearance:none;cursor:pointer;"><option>XS</option><option>S</option><option>M</option><option>L</option><option>XL</option><option>Único</option></select></div>
        </div>
        <div><div class="input-label">Marca</div><input class="input-field" id="up-brand" placeholder="Ej: Zara, H&M, Nike..." /></div>
        <div>
          <div class="input-label" style="margin-bottom:10px;">Estado</div>
          <div style="display:flex;gap:8px;flex-wrap:wrap;">
            <div class="chip selected" onclick="selectChip(this,'condition')">Nuevo</div>
            <div class="chip" onclick="selectChip(this,'condition')">Como nuevo</div>
            <div class="chip" onclick="selectChip(this,'condition')">Buen estado</div>
            <div class="chip" onclick="selectChip(this,'condition')">Usado</div>
          </div>
        </div>
        <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
          <div><div class="input-label">Precio (₡) *</div><input class="input-field" id="up-price" type="number" placeholder="0" min="0" /></div>
          <div><div class="input-label">Envío (₡)</div><input class="input-field" id="up-ship" type="number" placeholder="2000" min="0" /></div>
        </div>
        <p id="upload-error" style="color:#FF4757;font-size:13px;display:none;">Completa título y precio para publicar.</p>
        <button class="btn-primary" onclick="publishProduct()" style="margin-top:4px;">Publicar prenda 🚀</button>
      </div>
      <div style="height:24px;"></div>
    </div>
  </div>

  <!-- ======================== CHAT ======================== -->
  <div class="screen" id="screen-chat">
    <div style="background:var(--surface);padding:52px 16px 12px;border-bottom:1px solid var(--border);flex-shrink:0;">
      <div style="display:flex;align-items:center;gap:10px;">
        <button class="back-btn" onclick="go('screen-product')"><i class="ti ti-arrow-left"></i></button>
        <div class="avatar-placeholder" style="width:36px;height:36px;font-size:14px;">M</div>
        <div><div style="font-weight:600;font-size:14px;">@mia.shop</div><div style="font-size:12px;color:var(--green);">● En línea</div></div>
        <div style="margin-left:auto;"><i class="ti ti-dots-vertical" style="font-size:20px;color:var(--text-muted);cursor:pointer;"></i></div>
      </div>
    </div>
    <div style="background:var(--bg);padding:10px 14px;border-bottom:1px solid var(--border);flex-shrink:0;">
      <div style="background:var(--surface);border-radius:var(--radius-sm);padding:10px;display:flex;align-items:center;gap:10px;border:1px solid var(--border);">
        <div style="width:40px;height:40px;background:linear-gradient(135deg,#E8F0F8,#C5D5E8);border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:20px;flex-shrink:0;">👗</div>
        <div style="flex:1;"><div style="font-size:12px;font-weight:500;">Vestido midi floral</div><div style="font-size:13px;font-weight:700;color:var(--navy);">₡12,000</div></div>
        <div style="font-size:11px;color:var(--text-muted);">Talla M</div>
      </div>
    </div>
    <div class="scroll-area" style="padding:16px;display:flex;flex-direction:column;gap:10px;" id="chat-messages">
      <div style="text-align:center;font-size:11px;color:var(--text-muted);padding:8px 0;">Hoy, 9:32 AM</div>
      <div class="bubble-recv">Hola! ¿Está disponible el vestido? 😊</div>
      <div class="bubble-sent">Hola! Sí, está disponible. En excelente estado ✨</div>
      <div class="bubble-recv">¿Harías precio si compro el vestido y la bufanda?</div>
      <div class="bubble-sent">Claro! Los dos por ₡18,000 🌿 ¿Te parece?</div>
    </div>
    <div style="background:var(--surface);padding:10px 14px;border-top:1px solid var(--border);flex-shrink:0;">
      <div style="display:flex;gap:8px;align-items:flex-end;">
        <div style="flex:1;background:var(--bg);border-radius:22px;padding:10px 16px;border:1px solid var(--border);">
          <input id="chat-input" style="width:100%;border:none;background:transparent;font-size:14px;font-family:inherit;outline:none;color:var(--text);" placeholder="Escribe un mensaje..." onkeydown="if(event.key==='Enter')sendMsg()" />
        </div>
        <button onclick="sendMsg()" style="width:42px;height:42px;border-radius:50%;background:var(--green);border:none;display:flex;align-items:center;justify-content:center;cursor:pointer;flex-shrink:0;"><i class="ti ti-send" style="color:white;font-size:17px;"></i></button>
      </div>
    </div>
  </div>

  <!-- ======================== NOTIFICATIONS ======================== -->
  <div class="screen" id="screen-notifications">
    <div style="background:var(--surface);padding:52px 20px 12px;border-bottom:1px solid var(--border);flex-shrink:0;">
      <button class="back-btn" onclick="go('screen-home')" style="margin-bottom:12px;"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="font-size:22px;font-weight:600;">Notificaciones</div>
    </div>
    <div class="scroll-area">
      <div style="padding:8px 0;">
        <div style="display:flex;align-items:center;gap:12px;padding:14px 20px;border-bottom:1px solid var(--border);background:var(--green-light);cursor:pointer;">
          <div style="width:42px;height:42px;border-radius:50%;background:var(--green);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:18px;">❤️</div>
          <div style="flex:1;"><div style="font-size:14px;font-weight:500;"><span style="color:var(--navy);">@mia.shop</span> le dio me gusta a tu prenda</div><div style="font-size:12px;color:var(--text-muted);margin-top:2px;">Hace 5 min</div></div>
        </div>
        <div style="display:flex;align-items:center;gap:12px;padding:14px 20px;border-bottom:1px solid var(--border);cursor:pointer;" onclick="go('screen-chat')">
          <div style="width:42px;height:42px;border-radius:50%;background:var(--navy);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:18px;">💬</div>
          <div style="flex:1;"><div style="font-size:14px;font-weight:500;"><span style="color:var(--navy);">@ana.vintage</span> te envió un mensaje</div><div style="font-size:12px;color:var(--text-muted);margin-top:2px;">Hace 15 min</div></div>
        </div>
        <div style="display:flex;align-items:center;gap:12px;padding:14px 20px;border-bottom:1px solid var(--border);cursor:pointer;">
          <div style="width:42px;height:42px;border-radius:50%;background:#FFB800;display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:18px;">⭐</div>
          <div style="flex:1;"><div style="font-size:14px;font-weight:500;">Nueva reseña de <span style="color:var(--navy);">@laura.cr</span></div><div style="font-size:12px;color:var(--text-muted);margin-top:2px;">Hace 1 hora</div></div>
        </div>
        <div style="display:flex;align-items:center;gap:12px;padding:14px 20px;border-bottom:1px solid var(--border);cursor:pointer;">
          <div style="width:42px;height:42px;border-radius:50%;background:var(--green);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:18px;">📦</div>
          <div style="flex:1;"><div style="font-size:14px;font-weight:500;">Tu pedido está <span style="color:var(--green);">en camino</span></div><div style="font-size:12px;color:var(--text-muted);margin-top:2px;">Hace 3h · Correos CR</div></div>
        </div>
        <div style="display:flex;align-items:center;gap:12px;padding:14px 20px;cursor:pointer;">
          <div style="width:42px;height:42px;border-radius:50%;background:var(--green-light);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:18px;">💸</div>
          <div style="flex:1;"><div style="font-size:14px;font-weight:500;"><span style="color:var(--green);">¡Venta completada!</span> Recibes ₡12,000</div><div style="font-size:12px;color:var(--text-muted);margin-top:2px;">Ayer</div></div>
        </div>
      </div>
    </div>
  </div>

  <!-- ======================== PAYMENT ======================== -->
  <div class="screen" id="screen-payment">
    <div style="background:var(--surface);padding:52px 20px 12px;border-bottom:1px solid var(--border);flex-shrink:0;">
      <button class="back-btn" onclick="go('screen-product')" style="margin-bottom:12px;"><i class="ti ti-arrow-left"></i> Volver</button>
      <div style="font-size:22px;font-weight:600;">Confirmar compra</div>
    </div>
    <div class="scroll-area" style="padding:20px;">
      <div style="background:var(--bg);border-radius:var(--radius);padding:16px;margin-bottom:16px;">
        <div style="display:flex;gap:12px;align-items:center;">
          <div style="width:58px;height:58px;background:linear-gradient(135deg,#E8F0F8,#C5D5E8);border-radius:10px;display:flex;align-items:center;justify-content:center;font-size:26px;">👗</div>
          <div><div style="font-weight:500;">Vestido midi floral</div><div style="font-size:13px;color:var(--text-muted);">Talla M · Buen estado</div><div style="font-size:16px;font-weight:700;color:var(--navy);margin-top:4px;">₡12,000</div></div>
        </div>
      </div>
      <div style="background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:16px;margin-bottom:16px;">
        <div style="font-weight:600;margin-bottom:12px;">Método de pago</div>
        <div style="display:flex;flex-direction:column;gap:10px;">
          <div style="display:flex;align-items:center;gap:10px;padding:12px;border:2px solid var(--green);border-radius:var(--radius-sm);cursor:pointer;background:var(--green-light);">
            <div style="width:32px;height:22px;background:var(--navy);border-radius:4px;display:flex;align-items:center;justify-content:center;"><span style="color:white;font-size:10px;font-weight:700;">VISA</span></div>
            <span style="font-size:14px;font-weight:500;">•••• 4582</span>
            <div class="badge" style="margin-left:auto;font-size:10px;">Predeterminada</div>
          </div>
          <div style="display:flex;align-items:center;gap:10px;padding:12px;border:1px solid var(--border);border-radius:var(--radius-sm);cursor:pointer;">
            <span style="font-size:20px;">📱</span><span style="font-size:14px;">SINPE Móvil</span>
          </div>
        </div>
      </div>
      <div style="background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:16px;margin-bottom:24px;">
        <div style="font-weight:600;margin-bottom:12px;">Resumen</div>
        <div style="display:flex;flex-direction:column;gap:8px;font-size:14px;">
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--text-muted);">Precio</span><span>₡12,000</span></div>
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--text-muted);">Envío</span><span>₡2,000</span></div>
          <div style="display:flex;justify-content:space-between;"><span style="color:var(--text-muted);">Comisión LOOK</span><span>₡600</span></div>
          <div style="height:1px;background:var(--border);margin:4px 0;"></div>
          <div style="display:flex;justify-content:space-between;font-weight:700;font-size:16px;"><span>Total</span><span style="color:var(--navy);">₡14,600</span></div>
        </div>
      </div>
      <button class="btn-primary" onclick="go('screen-payment-success')">Pagar ₡14,600 🔒</button>
    </div>
  </div>

  <!-- ======================== PAYMENT SUCCESS ======================== -->
  <div class="screen" id="screen-payment-success">
    <div style="flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:40px 32px;text-align:center;background:white;">
      <div style="width:90px;height:90px;background:var(--green-light);border-radius:50%;display:flex;align-items:center;justify-content:center;margin-bottom:24px;">
        <i class="ti ti-check" style="font-size:44px;color:var(--green);"></i>
      </div>
      <div style="font-family:'Playfair Display',serif;font-size:28px;color:var(--navy);margin-bottom:8px;">¡Compra exitosa!</div>
      <div style="font-size:14px;color:var(--text-muted);line-height:1.6;margin-bottom:12px;">Tu pedido fue confirmado.</div>
      <div class="chip chip-green" style="margin-bottom:32px;">📦 Entrega en 3-5 días hábiles</div>
      <div style="background:var(--bg);border-radius:var(--radius);padding:16px;width:100%;margin-bottom:32px;text-align:left;">
        <div style="font-size:13px;color:var(--text-muted);margin-bottom:4px;">Número de pedido</div>
        <div style="font-size:16px;font-weight:600;color:var(--navy);">#LOOK-2024-4821</div>
      </div>
      <button class="btn-primary" onclick="go('screen-home')">Volver al inicio</button>
      <button class="btn-outline" style="margin-top:10px;" onclick="go('screen-notifications')">Ver mis pedidos</button>
    </div>
  </div>

</div><!-- end #app -->
</div><!-- end #app-wrap -->

<script>
// ====== STATE ======
let currentUser = JSON.parse(localStorage.getItem('look_user') || 'null');
let myFavorites = JSON.parse(localStorage.getItem('look_favorites') || '[]');
let myCloset = JSON.parse(localStorage.getItem('look_closet') || '[]');
let regData = {};
let currentProduct = null;
let productBackScreen = 'screen-home';

// ====== PRODUCTS DATA ======
const products = [
  { id:1, emoji:'👗', bg:'linear-gradient(135deg,#E8F0F8,#C5D5E8)', name:'Vestido midi floral', brand:'Zara', price:'₡12,000', size:'M', cat:'Vestido', condition:'Buen estado', ship:'₡2,000', desc:'Vestido midi floral de Zara, comprado el año pasado. Solo se usó dos veces, en perfectas condiciones. Estampado floral en tonos pastel, falda midi con vuelo. Perfecto para eventos o citas.' },
  { id:2, emoji:'👚', bg:'linear-gradient(135deg,#FFE8E8,#F5C0C0)', name:'Crop top H&M', brand:'H&M', price:'₡6,500', size:'XS', cat:'Blusa', condition:'Como nuevo', ship:'₡1,500', desc:'Crop top básico H&M en color blanco. Estado impecable, se usó muy poco. Ideal para el calor, combina con todo.' },
  { id:3, emoji:'🧥', bg:'linear-gradient(135deg,#D4E8D4,#A8C8A8)', name:'Blazer vintage 90s', brand:'Vintage', price:'₡18,500', size:'S', cat:'Jacket', condition:'Buen estado', ship:'₡2,500', desc:'Blazer vintage de los 90s en color verde oliva. Corte oversized perfecto para looks business casual o streetwear. Algunos signos de uso pero en buen estado general.' },
  { id:4, emoji:'👜', bg:'linear-gradient(135deg,#E8E0F0,#C8B8E8)', name:'Bolso Zara mini', brand:'Zara', price:'₡22,000', size:'Único', cat:'Bolso', condition:'Nuevo', ship:'₡2,000', desc:'Bolso mini Zara color lila, nunca usado. Tiene su etiqueta original. Perfecto para salidas, caben lo esencial: cartera, llaves y celular.' },
  { id:5, emoji:'👖', bg:'linear-gradient(135deg,#E8F4FF,#B8D8F8)', name:'Jeans Levi\'s 501', brand:"Levi's", price:'₡28,000', size:'28', cat:'Jean', condition:'Buen estado', ship:'₡2,000', desc:"Jeans Levi's 501 talla 28 en azul clásico. Muy cómodos y duraderos. Úsalos con cualquier top para un look infalible." },
  { id:6, emoji:'👟', bg:'linear-gradient(135deg,#F5E6D3,#E8C9A0)', name:'Nike Air Force 1', brand:'Nike', price:'₡35,000', size:'37', cat:'Zapato', condition:'Buen estado', ship:'₡3,000', desc:'Nike Air Force 1 talla 37 en blanco. Usadas con cuidado, suela limpia. Clásicas que nunca fallan.' },
];

// ====== NAVIGATION ======
function go(screenId) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  const el = document.getElementById(screenId);
  if (el) el.classList.add('active');
}

function switchTab(tab) {
  const map = { home:'screen-home', explore:'screen-explore', favorites:'screen-favorites', profile:'screen-profile' };
  if (tab === 'favorites') renderFavorites();
  if (tab === 'profile') renderProfile();
  if (tab === 'home') renderHome();
  if (map[tab]) go(map[tab]);
}

// ====== TIME ======
function updateTime() {
  const now = new Date();
  const t = now.getHours().toString().padStart(2,'0') + ':' + now.getMinutes().toString().padStart(2,'0');
  document.querySelectorAll('.status-time').forEach(el => el.textContent = t);
}
updateTime(); setInterval(updateTime, 30000);

// ====== CHIP HELPERS ======
function selectChip(el, group) {
  el.closest('div').querySelectorAll('.chip[onclick*="selectChip"]').forEach(c => c.classList.remove('selected'));
  el.classList.add('selected');
}
function toggleChip(el) { el.classList.toggle('selected'); }
function getSelected(container) {
  return [...container.querySelectorAll('.chip.selected')].map(c => c.textContent.trim());
}

// ====== AUTH ======
function doLogin() {
  const email = document.getElementById('login-email').value.trim();
  const pass = document.getElementById('login-pass').value.trim();
  const err = document.getElementById('login-error');
  if (!currentUser) { err.style.display = 'block'; return; }
  if (email === currentUser.email && pass === currentUser.password) {
    err.style.display = 'none';
    renderHome(); go('screen-home');
  } else { err.style.display = 'block'; }
}

// ====== REGISTER ======
function goReg2() {
  const name = document.getElementById('reg-name').value.trim();
  const email = document.getElementById('reg-email').value.trim();
  const pass = document.getElementById('reg-pass').value.trim();
  const err = document.getElementById('reg1-error');
  if (!name || !email || !pass) { err.style.display = 'block'; return; }
  err.style.display = 'none';
  regData.name = name; regData.email = email; regData.password = pass;
  const gels = document.querySelectorAll('#screen-reg1 [data-group="gender"]');
  regData.gender = [...gels].find(g => g.classList.contains('selected'))?.textContent || 'Otro';
  go('screen-reg2');
}

function goReg3() {
  regData.sizeTop = document.getElementById('reg-size-top').value;
  regData.sizeBot = document.getElementById('reg-size-bot').value;
  regData.sizeShoes = document.getElementById('reg-size-shoes').value;
  regData.styles = getSelected(document.getElementById('styles-wrap'));
  regData.cats = getSelected(document.getElementById('cats-wrap'));
  go('screen-reg3');
}

function previewAvatar(input) {
  if (!input.files[0]) return;
  const reader = new FileReader();
  reader.onload = e => {
    const ph = document.getElementById('reg-avatar-preview');
    const img = document.getElementById('reg-avatar-img');
    ph.style.display = 'none'; img.style.display = 'block'; img.src = e.target.result;
    regData.avatar = e.target.result;
  };
  reader.readAsDataURL(input.files[0]);
}

function goReg4() { go('screen-reg4'); }

function selectIntent(el) {
  document.querySelectorAll('#screen-reg4 [onclick="selectIntent(this)"]').forEach(e => e.style.borderColor = 'var(--border)');
  el.style.borderColor = 'var(--green)';
  regData.intent = el.querySelector('div:nth-child(2)').textContent;
}

function finishRegister() {
  regData.username = '@' + regData.name.split(' ')[0].toLowerCase() + '.look';
  const user = { ...regData };
  localStorage.setItem('look_user', JSON.stringify(user));
  currentUser = user;

  // Done screen
  const doneWrap = document.getElementById('done-avatar-wrap');
  const doneCheck = document.getElementById('done-check-icon');
  if (user.avatar) {
    doneWrap.innerHTML = `<img src="${user.avatar}" style="width:80px;height:80px;border-radius:50%;object-fit:cover;border:3px solid var(--green);margin-bottom:12px;" />`;
    doneCheck.style.display = 'none';
  }
  document.getElementById('done-welcome-name').textContent = `Bienvenida, ${user.name.split(' ')[0]} 🌿`;
  const chips = document.getElementById('done-chips');
  chips.innerHTML = '';
  [user.sizeTop ? `Talla ${user.sizeTop}` : '', ...(user.styles||[]).slice(0,3)].filter(Boolean).forEach(s => {
    const d = document.createElement('div'); d.className = 'chip chip-green'; d.textContent = '✓ ' + s; chips.appendChild(d);
  });
  go('screen-reg-done');
}

// ====== HOME ======
function renderHome() {
  if (currentUser) {
    document.getElementById('home-greeting').textContent = `Hola, ${currentUser.name.split(' ')[0]} 👋`;
  }
  const grid = document.getElementById('home-product-grid');
  grid.innerHTML = products.map(p => productCardHTML(p, 180)).join('');
}

function productCardHTML(p, h=170) {
  const faved = myFavorites.includes(p.id);
  return `<div class="product-card" onclick="openProduct(products[${p.id-1}])">
    <div style="position:relative;">
      <div style="height:${h}px;background:${p.bg};display:flex;align-items:center;justify-content:center;font-size:55px;">${p.emoji}</div>
      <button class="like-btn${faved?' liked':''}" style="position:absolute;top:8px;right:8px;" onclick="event.stopPropagation();quickFav(this,${p.id})"><i class="ti ti-heart"></i></button>
    </div>
    <div style="padding:10px;">
      <div style="font-size:13px;font-weight:500;color:var(--text);">${p.name}</div>
      <div style="font-size:15px;font-weight:600;color:var(--navy);margin:2px 0;">${p.price}</div>
      <div style="font-size:11px;color:var(--text-muted);">Talla ${p.size} · @mia.shop</div>
    </div>
  </div>`;
}

function filterCat(el, cat) {
  el.parentElement.querySelectorAll('.chip').forEach(c => c.classList.remove('selected'));
  el.classList.add('selected');
}

// ====== PRODUCT DETAIL ======
function openProduct(p, backScreen) {
  currentProduct = p;
  productBackScreen = backScreen || 'screen-home';
  document.getElementById('product-hero-emoji').textContent = p.emoji;
  document.getElementById('product-hero-bg').style.background = p.bg;
  document.getElementById('pd-name').textContent = p.name;
  document.getElementById('pd-brand').textContent = p.brand + ' · ' + p.condition;
  document.getElementById('pd-price').textContent = p.price;
  document.getElementById('pd-size').textContent = 'Talla ' + p.size;
  document.getElementById('pd-cat').textContent = p.cat;
  document.getElementById('pd-desc').textContent = p.desc;
  document.getElementById('pd-ship').textContent = p.ship;
  document.getElementById('product-back-btn').onclick = () => go(productBackScreen);
  // Fav heart
  const faved = myFavorites.includes(p.id);
  document.getElementById('fav-heart-icon').style.color = faved ? '#FF4757' : 'var(--text-muted)';
  go('screen-product');
}

function toggleFav(btn, p) {
  if (!p) return;
  const icon = btn.querySelector('i');
  if (myFavorites.includes(p.id)) {
    myFavorites = myFavorites.filter(id => id !== p.id);
    icon.style.color = 'var(--text-muted)';
  } else {
    myFavorites.push(p.id);
    icon.style.color = '#FF4757';
  }
  localStorage.setItem('look_favorites', JSON.stringify(myFavorites));
}

function quickFav(btn, pid) {
  if (myFavorites.includes(pid)) { myFavorites = myFavorites.filter(id => id !== pid); btn.classList.remove('liked'); }
  else { myFavorites.push(pid); btn.classList.add('liked'); }
  localStorage.setItem('look_favorites', JSON.stringify(myFavorites));
}

// ====== FAVORITES ======
function renderFavorites() {
  const grid = document.getElementById('fav-grid');
  const empty = document.getElementById('fav-empty');
  const count = document.getElementById('fav-count');
  const favProds = products.filter(p => myFavorites.includes(p.id));
  count.textContent = favProds.length + ' prendas guardadas';
  if (favProds.length === 0) { grid.innerHTML = ''; empty.style.display = 'block'; return; }
  empty.style.display = 'none';
  grid.innerHTML = favProds.map(p => `<div class="product-card" onclick="openProduct(products[${p.id-1}],'screen-favorites')">
    <div style="position:relative;">
      <div style="height:160px;background:${p.bg};display:flex;align-items:center;justify-content:center;font-size:52px;">${p.emoji}</div>
      <button class="like-btn liked" style="position:absolute;top:8px;right:8px;" onclick="event.stopPropagation();quickFav(this,${p.id});renderFavorites()"><i class="ti ti-heart"></i></button>
    </div>
    <div style="padding:10px;">
      <div style="font-size:13px;font-weight:500;">${p.name}</div>
      <div style="font-size:14px;font-weight:600;color:var(--navy);margin:2px 0;">${p.price}</div>
      <button class="btn-primary" style="margin-top:8px;padding:8px;font-size:12px;" onclick="event.stopPropagation();openProduct(products[${p.id-1}],'screen-favorites');setTimeout(()=>go('screen-payment'),50)">Comprar</button>
    </div>
  </div>`).join('');
}

// ====== PROFILE ======
function renderProfile() {
  if (!currentUser) return;
  const u = currentUser;

  // Avatar
  const ph = document.getElementById('profile-avatar-placeholder');
  const img = document.getElementById('profile-avatar-img');
  if (u.avatar) { ph.style.display='none'; img.style.display='block'; img.src=u.avatar; }
  else { ph.style.display='flex'; img.style.display='none'; ph.textContent = u.name.charAt(0).toUpperCase(); }

  document.getElementById('profile-display-name').textContent = u.name;
  document.getElementById('profile-handle').textContent = u.username || '@' + u.name.split(' ')[0].toLowerCase() + '.look';

  // Style chips
  const chips = document.getElementById('profile-style-chips');
  chips.innerHTML = '';
  const tags = [];
  if (u.sizeTop) tags.push('Talla ' + u.sizeTop);
  if (u.styles && u.styles.length) tags.push(...u.styles.slice(0,3));
  tags.forEach(t => { const d = document.createElement('div'); d.className='chip chip-green'; d.style.fontSize='11px'; d.style.padding='4px 10px'; d.textContent=t; chips.appendChild(d); });

  // Closet
  renderCloset();

  // AI greeting
  const first = u.name ? u.name.split(' ')[0] : '';
  const styles = u.styles && u.styles.length ? u.styles.slice(0,2).join(' + ').toLowerCase() : 'tu estilo único';
  document.getElementById('ai-greeting').textContent = `¡Hola ${first}! Basado en tu estilo ${styles}, hoy te tengo combinaciones perfectas 🌿`;
}

function changeProfilePhoto(input) {
  if (!input.files[0]) return;
  const reader = new FileReader();
  reader.onload = e => {
    currentUser.avatar = e.target.result;
    localStorage.setItem('look_user', JSON.stringify(currentUser));
    renderProfile();
  };
  reader.readAsDataURL(input.files[0]);
}

function showProfileTab(tab) {
  ['closet','sold','bought'].forEach(t => {
    document.getElementById('ptab-content-'+t).style.display = t===tab ? 'block' : 'none';
    const tabEl = document.getElementById('ptab-'+t);
    if(tabEl) { tabEl.classList.remove('active'); if(t===tab) tabEl.classList.add('active'); }
  });
}

function renderCloset() {
  const grid = document.getElementById('my-closet-grid');
  const empty = document.getElementById('closet-empty');
  document.getElementById('profile-sold-count').textContent = myCloset.length;
  document.getElementById('sold-msg').textContent = myCloset.length + ' prendas publicadas';
  if (myCloset.length === 0) { grid.innerHTML=''; empty.style.display='block'; return; }
  empty.style.display = 'none';
  grid.innerHTML = myCloset.map((item, i) => `<div class="product-card">
    ${item.photoSrc ? `<img src="${item.photoSrc}" style="width:100%;height:150px;object-fit:cover;" />`
      : `<div style="height:150px;background:var(--green-light);display:flex;align-items:center;justify-content:center;font-size:48px;">👗</div>`}
    <div style="padding:8px;">
      <div style="font-size:12px;font-weight:500;">${item.title}</div>
      <div style="font-size:13px;font-weight:600;color:var(--navy);">₡${Number(item.price).toLocaleString()}</div>
      <div style="font-size:11px;color:var(--text-muted);">Talla ${item.size}</div>
    </div>
  </div>`).join('');
}

// ====== UPLOAD PRODUCT ======
let uploadedPhotos = [];

function handleProductPhotos(input) {
  if (!input.files.length) return;
  const previews = document.getElementById('upload-previews');
  Array.from(input.files).slice(0, 8).forEach(file => {
    const reader = new FileReader();
    reader.onload = e => {
      uploadedPhotos.push(e.target.result);
      const img = document.createElement('img');
      img.src = e.target.result; img.className = 'photo-thumb';
      previews.appendChild(img);
    };
    reader.readAsDataURL(file);
  });
}

function publishProduct() {
  const title = document.getElementById('up-title').value.trim();
  const price = document.getElementById('up-price').value.trim();
  const err = document.getElementById('upload-error');
  if (!title || !price) { err.style.display='block'; return; }
  err.style.display='none';

  const item = {
    title, price,
    desc: document.getElementById('up-desc').value,
    cat: document.getElementById('up-cat').value,
    size: document.getElementById('up-size').value,
    brand: document.getElementById('up-brand').value,
    ship: document.getElementById('up-ship').value || '2000',
    photoSrc: uploadedPhotos[0] || null,
  };
  myCloset.unshift(item);
  localStorage.setItem('look_closet', JSON.stringify(myCloset));

  // Reset
  uploadedPhotos = [];
  document.getElementById('upload-previews').innerHTML = '';
  document.getElementById('up-title').value = '';
  document.getElementById('up-desc').value = '';
  document.getElementById('up-price').value = '';
  document.getElementById('up-ship').value = '';
  document.getElementById('up-brand').value = '';

  go('screen-profile'); renderProfile();
}

// ====== CHAT ======
function sendMsg() {
  const input = document.getElementById('chat-input');
  const msg = input.value.trim();
  if (!msg) return;
  const msgs = document.getElementById('chat-messages');
  const div = document.createElement('div');
  div.className = 'bubble-sent'; div.textContent = msg;
  msgs.appendChild(div);
  input.value = '';
  msgs.scrollTop = msgs.scrollHeight;
  setTimeout(() => {
    const r = document.createElement('div'); r.className='bubble-recv';
    r.textContent = '¡Perfecto! Te confirmo en un momento 😊';
    msgs.appendChild(r); msgs.scrollTop = msgs.scrollHeight;
  }, 1200);
}

// ====== INIT ======
function init() {
  renderHome();
  if (currentUser) renderProfile();
}
init();
</script>
</body>
</html>
