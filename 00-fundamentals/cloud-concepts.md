# Cloud Concepts

## What it is
What is a Resource?

A resource is an entity you can create, manage, or use within the cloud environment

Two main types of resources:
    1. Service-Level resources
        Compute instance VMs
        Cloud storage buckets
        Cloud SQL databases
    2. Account-level resources
        Organization, Folders, Project
        Logical grouping to organize service-level resources

Resource Hierarchy
    Organizational structure of resources
        Organization -> Folders -> Projects -> Resources

        Organization = Example Company
        Folders by
            Department IT
            Department further broken down by teams (Sales, Marketing, Customer Support)
            Teams broken down by Products (focus on specific features)
        Projects 
            Dev
            SIT
            QA
            UAT
            Prod
        Resources
            Compute Engine Instances
            App Engine Services
            Cloud Storage Buckets

Resource  
service level resources that are used to process workloads

Resource Management
configurations and access delegation to teams 

Domain
    primary identity of organization
    - defines which users to be associated with org
    - universally administer policy for users and devices
    - linked to either Google Workspace or Cloud Identity account

Organization
root node of the Google Cloud hierarchy of resources
    - define settings, permission, and policies for all projects, folders, resources, and Cloud Billing accounts it parents
    - ONLY ONE DOMAIN per Organization
    - Proactive and Reactive management through google cloud resources

Folders
    logical grouping of projects / folders

Projects
    logical grouping of service-level resources
    - projects can represent: teams, environments, organization units, business departments
    - basis for enabling services, APIs and IAM permissions
    A service-level resource can only belong to a single project

labels
    Categorize and filter resource with key/value pairs
    -great for cost tracking

3 suggested hierarchy architectures:
    1. environment-oriented
    2. function-oriented
    3. granular access-oriented

## Key concepts
- The hierarchy is **Org → Folders → Projects → Resources** — policies flow *down*; a child can only restrict, never grant more than its parent allows
- A service-level resource can only belong to **one project** — no sharing across projects directly
- **Projects** are the unit of billing, API enablement, and IAM scoping — not folders or orgs
- **Labels** (key/value pairs) are for metadata and cost tracking; they are NOT used for access control
- **Only one domain** per Organization — you cannot have two orgs under the same domain
- Folders can contain other folders or projects — nesting is allowed
- The **3 hierarchy architectures**: environment-oriented (dev/staging/prod), function-oriented (by business unit/team), granular access-oriented (fine-grained IAM by combining both)
- Without a Google Workspace or Cloud Identity account, projects sit under "No organization" — no org-level policies apply

## gcloud commands
```bash
gcloud compute instances list
gcloud iam roles list
```

## Gotchas / things that tripped me up
- **Labels ≠ Tags** — Labels are metadata for cost tracking and filtering; Tags in GCP are a separate concept used for network firewall rules and conditional IAM. Easy to conflate.
- **Policy inheritance is additive downward, not overridable upward** — if the Org denies something, a folder or project below cannot re-grant it. Many people expect child levels to be fully independent.
- **Only ONE org per domain** — if your company has multiple domains, each gets its own org. Merging orgs is not straightforward.
- **Projects are the billing unit** — you attach a billing account to a project, not to a folder or org. Forgetting this causes confusion when setting up budget alerts.
- The org node is **auto-created** when you link a Google Workspace or Cloud Identity account to GCP — you don't manually create it.

## Exam relevance
- **Hierarchy design** — "A company wants devs to access dev but not prod" → environment-oriented folder structure with IAM applied at the folder level
- **Cost tracking** — "How do you attribute GCP costs by team?" → apply labels to resources, then filter in Billing reports
- **Least privilege** — "Grant minimum access to a contractor for one project" → IAM binding at the project level, not org or folder
- **Policy inheritance** — "An org-level policy restricts external IP usage — can a project override it?" → No; org policies cascade down and cannot be loosened at lower levels
- **Resource sharing** — "Two teams need to share a VPC" → this is a Shared VPC scenario (covered in networking), not solved by hierarchy alone — resource hierarchy is about *management*, not direct resource sharing
- **Billing alerts** — "Set a budget alert for a specific department" → budget is set at the billing account level and can be scoped to a project or label filter

## Takeaway
The section focuses on how to organize projects from an administrative perspective. The goal is to keep the organization strategically permissions aligned. I can see where this is especially important for businesses that support hundreds and possibly thousands of users. It's important to have classifying permissions based on role. This helps grant based on least privileged access so permissions do not get extended beyond. 

If you are in marketing then you are allowed to access these set of projects. If you are part of Sales then you get these set of projects. It keeps all the permissions organized and prevents data leakage. I'm sure there are corner cases so I'd be curious on how GCP handles that. I'm sure there are special roles that can be granted to allow such flexibility in permissions.

