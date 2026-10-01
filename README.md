# Chronos-Builder-Template V2.2

Cloud Compilation Template — Fork to a private repository to generate your dedicated `.node` encrypted file with one click.

> 📌 **This repository is the cloud compilation template for Chronos Seal, only used for running GitHub Actions after forking.**
>
> For submitting Issues, viewing full documentation, or learning more about the project, please visit the main repository:
> [https://github.com/CrCLARE/Chronos-Seal](https://github.com/CrCLARE/Chronos-Seal)

> ⚠️ **This template is for RPG Maker MZ only.** RPG Maker MV uses pure JS lightweight protection, requires no cloud compilation, and is available directly in the main repository.

## How to Use

**1. Fork this repository (must be set to Private)**

**2. Trigger Actions**
Actions → Build Chronos Seal → Run workflow → Fill in parameters (game name, version number)

> The "Deadline" parameter has been removed in V2.2 — the expiration check mechanism is deprecated. Authors only need to fill in the game name and version number.

**3. Download Artifacts**
After compilation, download the artifact `chronos-seal-output-MZ` and unzip it to get:

- `decryptor.node` — Place this in the root directory of your game's distribution package.
- `encrypt_config.json` — Used during the local encryption stage. **Must be kept for the long term after the first build.**
- `author_secret.txt` — Keep offline. **Never place it inside the game package.**

**4. Delete the Forked Repository After Saving Credentials**
Delete the fork immediately after downloading to ensure logs and keys are not leaked.

> 🔑 **Important**: `encrypt_config.json` and `author_secret.txt` contain the master seed and are the foundation for all subsequent incremental patches. **Once lost, the published game can no longer be updated.** Please be sure to back them up offline (private repository / cloud drive / offline media).

## Compilation Pipeline

The GitHub Actions workflow in this template performs the following steps:

1. Validate input (game name 1-64 characters, version number alphanumeric with `. _ -` only)
2. Calculate build date
3. Install Node.js 18.x / Python 3.10 / node-gyp 9.4.0
4. **Generate fragmented seeds** (4 random seed segments + salt)
5. **Generate string cipher table** (`cs_str_table.h`, used to replace plaintext tags in the binary)
6. **Generate `build_config.h`** (Inject seeds, version number, and date as C++ compile-time constants)
7. Compile `decryptor.node` (VS2022 x64)
8. Generate `encrypt_config.json` and `author_secret.txt`
9. **Triple Artifact Verification**:
   - Artifact existence
   - No plaintext tags in the binary
   - Seeds correctly compiled into the binary
10. Upload artifact

Any verification step fails → Actions turns red directly, preventing "silent fallback to source code defaults".

## Relationship with the Main Repository

- [Chronos-Seal](https://github.com/CrCLARE/Chronos-Seal): Main repository, contains source code and complete documentation.
- [Chronos-Builder-Template](https://github.com/CrCLARE/Chronos-Builder-Template): This repository, the cloud compilation template.

## Version Compatibility

- This template corresponds to **Chronos Seal V2.2**.
- The encryption format of V2.2 is **incompatible** with V2.1. If you are upgrading from V2.1, you need to re-encrypt all assets.
- Older version games are not affected and will continue to run normally.

## Full Documentation

For detailed usage instructions, please check: [Chronos Seal Documentation](https://docs.crclare.top)

## License

This project is open-sourced under the MIT License. See the [LICENSE](LICENSE) file for details.

When using this software, please abide by the following conventions:

- ✅ Allowed: Integrate Chronos Seal into your commercial or free games, sell your game closed-source.
- ✅ Allowed: Modify the source code for your own projects.
- ✅ Allowed: Distribute under the terms of the MIT License.
- ❌ Strictly Prohibited: Selling the source code or compiled artifacts (`.node` files) of Chronos Seal as standalone commercial products.
- ❌ Strictly Prohibited: Selling the Chronos Seal itself after removing or hiding copyright notices.

**In simple terms: You can sell games that use Chronos Seal, but you cannot directly sell Chronos Seal itself.**

---

*This statement is a supplementary explanation to the MIT License and does not alter the authorization terms of the MIT License.*

**⭐ If this project has been helpful to you, please give the main repository a Star!**
