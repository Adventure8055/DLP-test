import google.cloud.dlp


def inspect_with_stored_infotype(
    project: str,
    stored_info_type_id: str,
    content_string: str,
) -> None:
    """Uses the Data Loss Prevention API to inspect/scan content using stored
    infoType.
    Args:
        project: The Google Cloud project id to use as a parent resource.
        content_string: The string to inspect.
        stored_info_type_id: The identifier of stored infoType used to inspect.
    """

    # Instantiate a client.
    dlp = google.cloud.dlp_v2.DlpServiceClient()

    # Convert stored infoType id into full resource id
    stored_type_name = f"projects/{project}/storedInfoTypes/{stored_info_type_id}"

    # Construct a custom info type dictionary using stored infoType.
    custom_info_types = [
        {
            "info_type": {"name": "STORED_TYPE"},
            "stored_type": {
                "name": stored_type_name,
            },
        }
    ]

    # Construct the inspection configuration dictionary.
    inspect_config = {
        "custom_info_types": custom_info_types,
        "include_quote": True,
    }

    # Construct the `item` to be inspected using stored infoType.
    item = {"value": content_string}

    # Convert the project id into a full resource id.
    parent = f"projects/{project}/locations/global"

    # Call the API.
    response = dlp.inspect_content(
        request={
            "parent": parent,
            "inspect_config": inspect_config,
            "item": item,
        }
    )

    # Print out the results.
    if response.result.findings:
        for finding in response.result.findings:
            print(f"Quote: {finding.quote}")
            print(f"Info type: {finding.info_type.name}")
            print(f"Likelihood: {finding.likelihood}")
    else:
        print("No findings.")
