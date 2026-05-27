# CHANGELOG


## v0.2.1 (2026-05-27)

### Bug Fixes

- Random shizz
  ([`f08cba8`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/f08cba8f403e706388dfd1bce83d4739d065be17))

### Chores

- Adding versioning stuff
  ([`ffe2909`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/ffe290965951f5a01d01cc032d4689b46858f239))

- Fixing current regex in version previw action
  ([`4115594`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/4115594a4a8926042d6c5ef7fcff0777897137d5))

- Locking uv
  ([`2602e9e`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/2602e9e6e9c8c8362e5f1276569afb13cbae90fe))


## v0.2.0 (2026-05-22)

### Bug Fixes

- Added default EppiAttributeSelectionType
  ([`e49f9ed`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/e49f9edae81f8d020093886d5917c9e6723f6bac))

Previously, the EppiAnnotationConverter would fail processing Eppi json annotations files with
  attributes missing AttributeType fields.

Now, we have an "Unspecified" EppiAttributeSelectionType that is selected by default.

- Attribute_csv creation clears file before generation
  ([`5e7a5df`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/5e7a5df8305bc28f6be760e9f53cc492251de7ef))

Previously, the export_attributes_csv_file method would write them to attribute successively by
  appending to a specified file. This can lead to an attributes csv having duplicated entries if the
  method is called again with the same file path (as one might via the CLI)

Now, we attempt to delete the specified via 'unlink' path before appending each attribute to it.

- Remove unnecessary TYPE_CHECKING import for Reference
  ([`3c2205c`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/3c2205ce028d30cf485af1970001ebba4c50b778))

- Reference is used at runtime in annotation_converter.py, not just for type hints - Remove
  TYPE_CHECKING conditional import pattern - Import Reference directly since it has no significant
  overhead - Addresses reviewer feedback about over-optimization

The TYPE_CHECKING pattern is only needed for imports that are: 1. Only used in type annotations (not
  at runtime) 2. Have significant import overhead or circular dependency issues Neither applies to
  Reference in this case.

- Resolve all pre-commit linting issues
  ([`9d5f297`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/9d5f29773f618a1b02428ce3605727522767bde2))

- Fix mypy type errors in annotation_converter.py - Refactor process_annotation_file method to
  reduce complexity - Fix ruff formatting and type annotation issues - All pre-commit hooks now pass

- Resolve Pydantic serialization error in Attribute models
  ([`3850b4a`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/3850b4ab024ab308350c20546d9a14870da27d6e))

- Change output_data_type from type objects to string representation in base Attribute class -
  Update EppiAttribute to use string 'bool' instead of bool type - Update annotation converter to
  use string 'bool' instead of bool type - Fixes 'Unable to serialize unknown type: <class 'type'>'
  error - Resolves serialization issue in save_processed_data method

- Restore item_id-based process_annotation_file after merge (Option 2)
  ([`d26949a`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/d26949a93f5d063a01b627f0438aff73a067e48d))

Remove reverted doc_title/documents_by_title logic from merge 4f80a8a. Use ItemId as unique key and
  reference.get('Codes', []) per reference so _create_pdf_to_title_mapping and
  _find_document_annotations are not needed.

- Restore set_attribute_type param and coerce output_data by attribute type
  ([`48925c2`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/48925c220df9fe64e4bc55b6f56ad57404ea458f))

- Add set_attribute_type parameter to process_annotation_file (lost in merge) - Coerce empty
  output_data to type-appropriate defaults when ItemAttributeFullTextDetails is absent (fixes
  ValidationError for integer/list/dict in item_id-based flow) - Use
  AttributeType.to_python_type()() for cleaner coercion logic - Remove duplicate
  _extract_attributes_from_codesets definition

Co-authored-by: Cursor <cursoragent@cursor.com>

- Support non-boolean data types in extraction and evaluation pipeline
  ([`08e9fd1`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/08e9fd178f1087c917e9f15b3ae3ebe561423f52))

- Add default values for STRING, INTEGER, FLOAT, DICT in get_attribute_annotation (previously only
  BOOL and LIST were handled, causing ValueError for string attributes) - Fix detached f-string bug
  in error message for missing annotations - Split evaluation metrics into BINARY_METRICS and
  NON_BINARY_METRICS so binary-only sklearn metrics (recall, precision, f1) are not applied to
  string attributes - Add exact_match metric for non-binary attribute types - Select metrics per
  attribute type via get_metrics_for_attribute_type() - Gracefully handle metric failures with
  warnings and N/A display

Made-with: Cursor

- Use string literals for forward references instead of __future__ annotations
  ([`19dcdec`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/19dcdec9fc6dd24df5daadffc9dacedacd1392ed))

- Remove 'from __future__ import annotations' - Use string literals for forward references:
  'Attribute', 'Document', 'GoldStandardAnnotation' - Prevents potential NameError when TYPING=False
  or in certain contexts - Addresses reviewer feedback about forward reference handling - More
  explicit and robust approach to forward references

This ensures forward references work correctly in all Python contexts without relying on __future__
  import behavior.

### Chores

- Align parser.py with main for PR
  ([`aa40130`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/aa40130e429fff4d3296a1c787b9b7d76263189c))

Restore filename_base line to match origin/main (revert branch-only split tweak).

Made-with: Cursor

- Drop incidental pipeline churn; keep parser E501 fix minimal
  ([`6f2b7ee`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/6f2b7ee0969dcbdc21d627bf0fa3095364b7c36d))

- Restore deet/data_models/pipeline.py to match main (ruff-format-only diff reverted). - parser:
  same logic as main (first line of text) with a two-line form to stay under 88 cols. - Remove
  [tool.mypy] python_version from pyproject (revert prior config addition).

Made-with: Cursor

- Restore parser.py to match main (no branch-local edits)
  ([`efde321`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/efde321bc2865daef8af790ee6a8db0644abe793))

Our earlier parser tweak was only to satisfy a local pre-commit run over unrelated files; upstream
  already linted clean. Drop the diff entirely.

Made-with: Cursor

### Documentation

- Clarify converter file configs; base output dir; use of Outfiles registery.
  ([`db918b7`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/db918b72b8918dda3676e047d9949c829436c43a))

- Update README example to use generic paths
  ([`eccb636`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/eccb636a9c79dedba54e7f50882a2c5caf0dc0a7))

- Change specific file paths to generic input_path/output_path in example - Improve documentation
  clarity

### Features

- Add directory-based batch processing to pipeline scripts
  ([`f3fa9c6`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/f3fa9c6e8027d4f1dd615348eff7e52fa1dbb95e))

- Update LLMDataExtractor.extract_from_documents to return dict[str, list[GoldStandardAnnotation]]
  keyed by file path - Update _save_results to handle dictionary structure with backward
  compatibility - Add process_directory function to all pipeline scripts for batch processing -
  Support --pdf_dir and --markdown_dir arguments for directory input - Auto-detect directories when
  -p or -m point to directories (backward compatible) - Auto-generate output path if not provided
  (follows same pattern as single-file mode) - Process files sequentially with per-file error
  handling - Check for existing markdowns in markdown_dir and skip parsing if found - Save parsed
  markdowns to markdown_dir when provided - Output format: single JSON file with dictionary
  structure {file_path: [annotations]} - Maintain full backward compatibility with single file input
  - Add comprehensive logging for file processing status - Fix mypy type annotations for
  results_json variable

- Add document_label field to Document class
  ([`0741993`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/074199369c5c7991f3b61990e9ed7a2b2e399966))

- Add document_label field to Document class for human-readable identification - Follows same
  pattern as Attribute class (attribute_id + attribute_label) - Update annotation converter to
  populate document_label from Title field - Maintains consistency between Document and Attribute
  models - Addresses reviewer feedback about implementing similar pattern to attribute_id

Now Document has: - document_id: unique identifier - document_label: human-readable label (from
  Title) - name: document name - citation: reference information

- Add LLM evaluation system and simplify codebase
  ([`c86d402`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/c86d40281a2b45907c71a26fd7122bc22ee328ca))

- Add LLM evaluation system with Azure OpenAI and OpenAI support - Add EPPI data models and
  annotation converter - Add simple LLM evaluation script with comprehensive logging - Update README
  with clear usage instructions - Replace large test PDF with smaller one for faster testing - Add
  JSON files and data directories to .gitignore - Remove complex multi-file LLM evaluation system in
  favor of simple approach

- Extraction models, tokenisation cost handling, and tests
  ([`6eb3104`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/6eb310417471877ef4119747eb15fce2b7e57349))

- Wire DocumentExtractionResult cost via estimate_cost_usd and merge helper in tokenisation - Add
  LitellmModelNotMappedError for unmapped litellm registry on max tokens - Soften estimate_cost_usd
  and count_tokens for provider/registry failures; remove blind except in _call_llm - Extend
  settings and env.example for context token limits - Add unit tests for tokenisation and extraction
  data models

Made-with: Cursor

- **#132**: Add non-boolean output support for EPPI extraction
  ([`c59fb61`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/c59fb6177332fd723004c688e9c6b291fd76c333))

- Add AttributeType.parse() classmethod in base.py for parsing type names from CSV/JSON (string,
  bool, integer, etc.) with pass-through for existing AttributeType and None handling - Update
  _import_prompts_csv_file in eppi.py to read output_data_type from CSV column and set attribute
  type via AttributeType.parse() - Coerce empty string to False for BOOL attributes in converter
  when ItemAttributeFullTextDetails is absent (backward compatibility) - Add unit tests for
  AttributeType.parse(), CSV import of output_data_type, and fix converter test for non-boolean
  outputs

Co-authored-by: Cursor <cursoragent@cursor.com>

- **#145**: Add pdfminer as fast PDF parser option and default
  ([`59506f3`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/59506f36e0f07656a457155bcbcf2ec2ffe75060))

- Add PdfminerParser using pdfminer.six for fast text extraction - Set PdfminerParser as default PDF
  parser (replacing MarkerParser) - Add pdfminer.six dependency and mypy override - Add unit tests
  for PdfminerParser - Lint fixes: test_eppi imports, ruff format

Co-authored-by: Cursor <cursoragent@cursor.com>

- **eval**: Extend gold/LLM comparison CSV and tidy verbatim-fuzzy
  ([`dcc21e1`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/dcc21e135e73175bf43027d22e4f563e263d127e))

- Add attribute_presence, human_additional_text, item_attribute_full_text_details - Keep
  llm_verbatim_text and both verbatim fuzzy match columns - Remove redundant human_verbatim_text
  column; score human fuzzy from human_additional_text - Update unit test for new CSV shape

Made-with: Cursor

### Refactoring

- Batch-only prompt outfile, dict format, ruff/test fixes
  ([`36f8049`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/36f8049ab4869c6b5fa7ec3c81897b1cd157223f))

- Remove prompt_outfile from extract_from_document; only extract_from_documents writes prompt
  payloads - Write full_prompt_payload.json as single dict (path -> messages), same shape as
  llm_extractions.json - Fix ruff: PT018 split assertions, A006/ARG005 parser test lambdas, apply
  format - All 141 tests passing

- Clarify destiny based reference builder names and improve mapping guildlines for
  _build_destiny_reference_dict_from_row()
  ([`73e8d8b`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/73e8d8b04e989a6993c4ad3fd6304c223d0b693f))

- Make AttributesList iterable with __iter__ method
  ([`1d17e0d`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/1d17e0d8c3a781f2c5d329e7310ce95bbe832251))

- Add __iter__ method to make AttributesList directly iterable - Simplify to_list() method to use
  __iter__ via list(self) - Enables more Pythonic usage: list(my_attribute_list) - Maintains
  backward compatibility with existing to_list() method - Addresses reviewer feedback for more
  Pythonic design

Now users can do: my_attribute_list = AttributesList(**data) the_list = list(my_attribute_list) #
  Direct iteration or the_list = my_attribute_list.to_list() # Explicit method

- Move annotation converter CLI to scripts directory
  ([`13b52ec`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/13b52ec713fa50d84403c76e05547c670ddb001e))

- Move CLI functionality from app/processors/annotation_converter.py to
  app/scripts/annotation_converter.py - Keep AnnotationConverter class in processors for potential
  reuse - Update README.md to reflect new script location and usage - Add proper path handling for
  script execution - Remove unused logger import from processors module

Addresses reviewer feedback about module vs script separation.

- Move missing-annotation defaults to AttributeType
  ([`2c1d563`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/2c1d5638435b84697aca698277a02d753540100b))

- Add AttributeType.missing_annotation_default() with docs on Enum._missing_ - Use it from
  GoldStandardAnnotatedDocument.get_attribute_annotation - Fix CSV error log message concatenation -
  Document CSV filtering in extract-data CLI help - Add unit tests for defaults and fresh list/dict
  instances

Made-with: Cursor

- Remove document parameter from extraction methods
  ([`7d8e2c8`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/7d8e2c832a4964197961518002f4624da9bf9f24))

- Remove document parameter from extract_from_document method - Remove documents parameter from
  extract_from_documents method - Remove document parameter from _prepare_context method - Remove
  unused Document import - Update method signatures and docstrings accordingly - Comment out
  document loop and abstract context handling for future reference

- Remove LLM evaluation functionality from data models PR
  ([`03f060c`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/03f060ca5323cc7cd7fbfb5748cf41404c9c1f12))

- Remove simple_llm_eval.py script - Remove LLM-related dependencies (litellm, openai,
  python-dotenv) - Remove LLM evaluation section from README - Clean up LLM references in
  documentation - Keep only data models and annotation converter functionality

LLM functionality moved to separate branch: sagar-llm-evaluation

- Remove PDF-to-title mapping and use nested reference structure
  ([#115](https://github.com/nikloynes/data-extraction-evaluation-toolkit/pull/115),
  [`15b2ba5`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/15b2ba5d850948e338179ba047dc033e81e8366f))

- Remove _create_pdf_to_title_mapping() method - Remove _find_document_annotations() method -
  Refactor process_annotation_file() to process annotations directly from nested reference structure
  - Use ItemId instead of doc_title for unique document identification - Annotations are already
  nested within their parent Reference, making matching logic redundant

This simplifies the code by removing ~46 lines of unnecessary matching logic and makes the code more
  efficient and maintainable.

- Simplify models/__init__.py to remove over-engineered imports
  ([`c8168bd`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/c8168bd6b9954c85e10a8a1fe89156d9355eaebf))

- Remove explicit class imports and __all__ list - Use standard Python module import pattern - Add
  helpful comments showing usage examples - Code already uses standard imports (from app.models.base
  import ...) - Addresses reviewer feedback about over-engineering

This follows the principle of 'explicit is better than implicit' and reduces maintenance overhead
  while keeping the API clear.

- **eppi**: Extract AdditionalText mapping; align Ruff config with main
  ([`5d8fbbd`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/5d8fbbddb76ee5479114344a2846d979926601d6))

- Add deet/processors/eppi_additional_text_mapping.py for eppi_output_data_from_eppi_fields (shared
  by converter and ProcessedEppiAnnotationData without circular imports). - Import mapping at module
  level in processed_gold_standard_annotations; drop lazy import. - Remove per-file PLC0415 ignore
  for processed_gold_standard_annotations (match main). - Document export_llm_comparison CSV
  columns; inline gold annotation lookup; trim converter.

Made-with: Cursor

- **eppi**: Fold additional-text mapping into converter
  ([`fff78f1`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/fff78f13fec3855a7c1e78aca538ab3afd9480fa))

- Inline eppi_output_data_from_eppi_fields branches in eppi_annotation_converter; remove
  eppi_additional_text_mapping module - Mark lazy import noqa PLC0415 in ProcessedEppiAnnotationData
  (avoid circular import)

Made-with: Cursor

- **evaluation**: Drop exact_match; map metrics by AttributeType
  ([`7dcac8e`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/7dcac8e3d4f99762b60d45d5bbd218ccd0b36d5d))

Remove redundant exact_match (equivalent to accuracy_score for aligned predictions). Add
  METRICS_BY_ATTRIBUTE_TYPE so each AttributeType selects its metric set explicitly, enabling
  per-type extensions without if/elif.

Made-with: Cursor

- **evaluation**: Spell out INTEGER_METRICS like STRING_METRICS
  ([`8cdbb67`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/8cdbb67bba5019af8a28964b9439076a5ed5f5fc))

Use an explicit metric dict for integers so future per-type changes show clear diffs instead of
  mutating a shared STRING_METRICS alias.

Made-with: Cursor

- **evaluation**: Split metrics by type; defer float/list/dict
  ([`6631e2e`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/6631e2e2f99d92cc7c7e371cf3017e625401e849))

- STRING_METRICS and INTEGER_METRICS keep accuracy (discrete per-value match). - FLOAT_METRICS left
  empty until regression metrics (e.g. MAE) exist. - LIST_METRICS and DICT_METRICS empty until
  structured comparison metrics exist. - Document empty sets in get_metrics_for_attribute_type;
  custom metrics still apply.

Made-with: Cursor

- **extractor**: Extract_from_documents helpers + batch-only save
  ([`469d72a`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/469d72a0871aeff6eae44b9f7d9cd153700eb577))

- Add _get_document_input_files, _input_file_to_md_path, _write_json_if_path - Use helpers in
  extract_from_documents; remove list-format backward compat in _save_results - Log context type in
  _prepare_context; optional prompt_outfile unchanged - Add unit tests for the three helpers

- **parser**: Move pdfminer logic into PdfMinerParser, add EmptyPdfExtractionError
  ([`d9ed5fb`](https://github.com/nikloynes/data-extraction-evaluation-toolkit/commit/d9ed5fb48e3771649bca85afcb7311210bfd2167))

- Move _parse_with_pdfminer logic into PdfMinerParser.parse() for consistency - Use class-level
  LAParams constant for efficiency - Add EmptyPdfExtractionError when PDF has no extractable text -
  Update tests: mock PdfminerParser.parse, add empty-extraction test - Fix ruff lint (TRY003, EM101,
  ANN202, E501)

Co-authored-by: Cursor <cursoragent@cursor.com>
