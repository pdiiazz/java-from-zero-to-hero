# LTS review checklist

## Before

- [ ] Add the new LTS to `docs/java-version-matrix.md`
- [ ] List JEPs and JLS changes since the previous LTS
- [ ] Identify topics whose `since:` or `java_version:` changes

## During

- [ ] Update affected topic frontmatter
- [ ] Update affected topic bodies and references
- [ ] Check every code snippet against the new LTS

## After

- [ ] Move `Baseline` to the new LTS in `docs/java-version-matrix.md` and set `java_version` in `_template.md`
- [ ] Check every link in `README.md` resolves
