# frozen_string_literal: true

task default: 'lint:all'

namespace :lint do
  desc 'Lint and format'
  task all: %i[format rubocop eslint]

  desc 'Run rubocop'
  task :rubocop do
    sh 'rubocop -a'
  end

  desc 'Format sources'
  task format: :deps do
    sh 'cargo fmt -- --color=auto'
    sh "yarn prettier --write '**/*'"
    sh "yarn prettier --prose-wrap always --write '**/*.md' '*.md'"
    sh 'node scripts/clang-format.js'
  end

  desc 'Run eslint'
  task eslint: :deps do
    sh 'yarn eslint --fix .'
  end

  desc 'Check markdown links'
  task links: :deps do
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json CONTRIBUTING.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json README.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json artichoke-backend/README.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json artichoke-backend/src/extn/stdlib/gen/README.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json artichoke-backend/vendor/README.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json artichoke-core/README.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json doc/artichoke-design-and-goals.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json doc/build.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json doc/ruby-spec.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json spec-runner/README.md'
    sh 'yarn run markdown-link-check --config .github/markdown-link-check.json spec-runner/vendor/README.md'
  end

  desc 'Install linting dependencies'
  task :deps do
    sh 'yarn install --frozen-lockfile'
  end

  desc 'Lint with restriction pass (unenforced lints)'
  task :restriction do
    sh 'cargo clippy -- ' \
      '-W clippy::dbg_macro ' \
      '-W clippy::get_unwrap ' \
      '-W clippy::indexing_slicing ' \
      '-W clippy::option_expect_used ' \
      '-W clippy::option_unwrap_used ' \
      '-W clippy::panic ' \
      '-W clippy::print_stdout ' \
      '-W clippy::result_expect_used ' \
      '-W clippy::result_unwrap_used ' \
      '-W clippy::todo ' \
      '-W clippy::unimplemented ' \
      '-W clippy::unreachable'
  end
end

desc 'Generate Rust API documentation'
task :doc do
  sh 'rustup run --install nightly cargo doc --workspace --no-deps'
end

desc 'Generate Rust API documentation and open it in a web browser'
task :'doc:open' do
  sh 'rustup run --install nightly cargo doc --workspace --no-deps --open'
end

desc 'Run enforced ruby/spec suite'
task :spec do
  sh 'cargo run -q -p spec-runner -- spec-runner/enforced-specs.yaml'
end
