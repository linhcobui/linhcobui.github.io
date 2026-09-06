Recommendation: install Ruby 2.7 (Windows) and run Bundler 1.17.3

Why: The project depends on older Bundler/Jekyll versions incompatible with Ruby 4. Porting the gemspec
and dependencies to modern Ruby is higher-risk and more invasive. Installing Ruby 2.7 reproduces the
original runtime and is the safest, fastest fix.

Quick steps (copy-paste into an elevated PowerShell prompt):

1) Download RubyInstaller-2.7 (example uses 2.7.8):

```powershell
$url = 'https://github.com/oneclick/rubyinstaller2/releases/download/RubyInstaller-2.7.8-1/rubyinstaller-2.7.8-1-x64.exe'
$out = "$env:USERPROFILE\Downloads\rubyinstaller-2.7.8-1-x64.exe"
Invoke-WebRequest -Uri $url -OutFile $out
Start-Process -FilePath $out -Wait
```

Follow the GUI installer: check "Add Ruby executables to your PATH" and install MSYS2 when prompted (or run
the separate `ridk install` afterwards).

2) From a new PowerShell (after installer finishes):

```powershell
ruby -v
gem install bundler -v 1.17.3
cd "$env:USERPROFILE\linhcobui.github.io"
bundle _1.17.3_ install
```

If the installer offered MSYS2, you may need to run `ridk install` and accept defaults before gem native
extensions build successfully.

If you prefer a non-GUI path (WSL + rbenv) or want me to download the installer and run steps for you, tell me
which option you want and I will proceed.
