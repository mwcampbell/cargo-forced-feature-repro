# Cargo forced feature repro

This repository is a minimal test case that reproduces an apparent bug in Cargo, which causes a build dependency of a crate to force runtime features to be enabled despite the user's wishes.

Notice that in Cargo.toml, default_features is set to false for env_logger. But when you build the binary, env_logger has all features enabled anyway, as is evident from the binary size and the output of cargo-tree.

This is caused by the dependency on speech-dispatcher, which has an indirect build dependency on bindgen, which depends on env_logger with default features. This dependency forces env_logger in the final program to have default features even when built on a non-Linux platform (the dependency on speech-dispatcher is conditional). If you remove the dependency on speech-dispatcher, then the optional features of env_logger are disabled as expected.
