## Task: Integrate Blockchain-Based Certification System into Course Library Application
 
This task involves integrating the newly developed blockchain-based certification system into the existing Orion Path course library application. The goal is to enable users to manage their Algorand addresses, automatically issue certificates upon course completion, display earned certificates, and provide a public certificate verification mechanism.

### Implementation Details:

1.  **User Profile Integration (Algorand Address Management)**:
    *   **Objective**: Allow authenticated users to link and manage their Algorand wallet address within their profile.
    *   **Location**: Modify the `AccountSettings` component in `src/components/profile/account-settings.tsx`.
    *   **Changes**:
        *   Add a new input field for `algorand_address`.
        *   When the user saves their profile settings, use `certificateOperations.updateUserAlgorandAddress(user.id, algorandAddress)` to store the address in the Supabase `user_profiles` table.
        *   Ensure proper validation for the Algorand address format.

2.  **Automated Certificate Issuance on Course Completion**:
    *   **Objective**: Automatically generate and issue a blockchain-backed certificate when a user successfully completes a course.
    *   **Location**: Modify the `updateCourseProgress` function within `dbOperations` in `src/lib/supabase.ts`.
    *   **Changes**:
        *   Update the `updateCourseProgress` function signature to accept an optional `examinationResults` parameter of type `ExaminationTranscript`.
        *   When `completed` is `true` in `updateCourseProgress`, call `getCertificateAPI().onExaminationCompletion(user.id, courseId, examinationResults)`.
        *   You will need to ensure that the `examinationResults` object is populated with realistic data (even if mocked for now) when a course is marked as completed. This data should conform to the `ExaminationTranscript` interface.
        *   Add appropriate error handling and toast notifications for the certificate issuance process.

3.  **Display User's Earned Certificates**:
    *   **Objective**: Create a new page to display all earned certificates.
    *   **Location**: Create a new page and add it to sidebar menu.
    *   **Changes**:
        *   Create a new React component, e.g., `src/components/profile/my-certificates.tsx`.
        *   This component should fetch the current user's certificates using `getCertificateAPI().getStudentCertificates(user.id)`.
        *   Use the `CertificateViewer` component (from `src/components/certificates/certificate-viewer.tsx`) to render each fetched certificate.
        *   Handle loading and empty states gracefully within `my-certificates.tsx`.
        

4.  **Public Certificate Verification Page**:
    *   **Objective**: Provide a public page where anyone can verify a certificate using its ID and optionally a student's Algorand address.
    *   **Location**: Add a new route in `src/App.tsx`.
    *   **Changes**:
        *   Create a new page component, e.g., `src/pages/verify-certificate.tsx`.
        *   This page should render the `CertificateVerificationWidget` component (from `src/components/certificates/certificate-verification-widget.tsx`).
        *   Add a navigation link to this page (e.g., in the footer or a public section).

### Required Interfaces:

The following TypeScript interfaces are crucial for this integration. Ensure all data structures conform to these definitions:

```typescript
export interface CertificateData {
  certificateId: string;
  studentId: string;
  courseId: string;
  courseName: string;
  score: number;
  maxScore: number;
  examinationDate: string;
  transcriptHash: string;
  metadata: CertificateMetadata;
  issuerAddress: string;
  timestamp: number;
  status: 'active' | 'revoked';
  blockchainTxId?: string;
}

export interface CertificateMetadata {
  examinationType: 'quiz' | 'assessment' | 'final';
  modulesCompleted: number;
  totalModules: number;
  completionTime: number; // in minutes
  difficultyLevel: number;
  keywords: string[];
  learningObjectives: string[];
  passingScore: number;
  achievementLevel: 'bronze' | 'silver' | 'gold' | 'platinum';
}

export interface ExaminationTranscript {
  studentId: string;
  courseId: string;
  moduleResults: ModuleResult[];
  totalScore: number;
  maxPossibleScore: number;
  completionDate: string;
  timeSpent: number;
  examType: 'practice' | 'final';
}

export interface ModuleResult {
  moduleIndex: number;
  moduleName: string;
  score: number;
  maxScore: number;
  questionsAnswered: number;
  correctAnswers: number;
  timeSpent: number;
  topics: TopicResult[];
}

export interface TopicResult {
  topicIndex: number;
  topicName: string;
  score: number;
  maxScore: number;
  understanding: 'poor' | 'fair' | 'good' | 'excellent';
}

export interface CertificateVerificationResult {
  isValid: boolean;
  certificateData?: CertificateData;
  error?: string;
  verificationTimestamp: number;
  blockchainConfirmed: boolean;
}

export interface BlockchainConfig {
  network: 'mainnet' | 'testnet' | 'sandbox';
  algodServer: string;
  algodPort: number;
  algodToken: string;
  appId: number;
  creatorAddress: string;
  creatorMnemonic: string;
}
