resources(name="package_data", sources=["pyproject.toml", "README.md"])

python_distribution(
    name="package",
    dependencies=[
        ":package_data",
        "./a:a",
    ],
    provides=python_artifact(
        name="a",
        version="0.1.0",
        long_description_content_type="markdown",
    ),
    long_description_path="src/py/a/README.md",
)
