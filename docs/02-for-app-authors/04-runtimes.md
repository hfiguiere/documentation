# Runtimes

Flathub was originally launched with a runtime/SDK originally created by the Flatpak developers, which aimed to provide the common dependencies used by most Linux desktop apps. It's now maintained independently by the [freedesktop-sdk](https://gitlab.com/freedesktop-sdk/freedesktop-sdk) project. It is used as the basis of a GNOME and KDE specific runtimes which derived from the same basic binaries and shared the extension points, and layers on dependencies specific to those different environments.

It's preferable from a user perspective for Flatpak apps to be consolidated around the same or similar/derived runtimes because it decreases download sizes, and due to the ostree deduplication it increases the re-use of binaries and data files both in disk and in memory. For developers, it also means tooling is consistent, documentation can be more precise, SDK extensions can be provided that are usable/useful across most apps, and it also ensures that the Flatpak technology remains independent/agnostic of any specific Linux OS or distribution. For these reasons, the freedesktop-sdk and derived runtimes are still preferred.

However, in order to give developers the choice of the most suitable environment and tools for their apps, and allow different choices and guarantees to be made about lifecycle, stability, security, licensing and provenance, Flathub is allowing external runtime submissions in source or binary form. The below requirements, subject to updates by the Flathub team from time to time, must be met by any runtime hosted on Flathub.

## Currently hosted runtimes

- [org.freedesktop](https://gitlab.com/freedesktop-sdk/freedesktop-sdk)
- [org.gnome](https://gitlab.gnome.org/GNOME/gnome-build-meta)
- [org.kde](https://invent.kde.org/packaging/flatpak-kde-runtime)
- [io.elementary](https://github.com/elementary/flatpak-platform)

## Requirements

- **Release frequency**: major releases no more frequently than 6 monthly, to avoid bloating the repository both on Flathub and on user systems
- **Support lifecycle**: each major release must be updated with critical (eg remotely exploitable, sandbox escapes, etc) security updates for a period of at least 12 months
- **EOL**: runtimes no longer receiving updates must be released with an `eol` marker to make the user aware
- **Development tooling**: the SDK must contain all of the tools usually necessary to build applications that target the runtime, without external infrastructure - ie a developer on Flathub must be able to submit a flatpak-builder manifest that is able to make use of your runtime for their app
- **First-party uploads**: runtimes must be built on Flathub from source, or can be uploaded using flat-manager by the developer or organization that is responsible for building the binaries. uploads of binaries obtained from an unrelated third party are not normally permissible.
- **Reproducibility**: the ostree commit for the runtime must contain the relevant metadata (eg git hashes etc) necessary to fetch the corresponding source for all of the constituent modules included within the runtime
- **Architectures**: 64-bit Intel (`x86_64`) and 64-bit ARM (`aarch64`) are required
- **CI/CD workflow**: Flathub uploads must be automated as a part of the regular build/update process for the runtime, so that security updates are not delayed
- **GL drivers**: runtimes must use glvnd for loading GL drivers, and provide compatibility with the `org.freedesktop.Platform.GL//1.4` extension point for external GL drivers, to ensure broadest compatibility with user systems. your runtime must support the `org.freedesktop.Platform.GL.nvidia-*-*` extensions published on Flathub, and must also support the host system providing its own GL stack through an unmanaged `org.freedesktop.Platform.GL.host` extension. your runtime may bundle additional drivers such as Mesa and/or provide them through your own extension.
- **Usage**: after a grace period of at least 6 months, runtimes which are not used by any apps on Flathub may be retired at the discretion of the Flathub team

## Submission process

Runtime developers may submit a ticket to the Flathub issue tracker requesting that their runtime be added to Flathub. The Flathub team will review the request and may request changes to the runtime to meet the above requirements. Once the runtime is approved, the Flathub team will add the runtime to the Flathub repository and publish it to the Flathub website.
