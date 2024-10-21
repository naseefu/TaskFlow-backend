package com.task.repository;

import java.util.List;
import java.util.Optional;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import com.task.entity.Project;
import com.task.entity.Team;

@Repository
public interface ProjectRepository extends JpaRepository<Project, Long> {

	List<Project> findByUserId(Long user_id);

	List<Project> findByTeamId(Long teamId);
	
	boolean existsByUserIdAndTeamId(Long userId,Long teamId);
	
	boolean existsByIdAndUserIdAndTeamId(Long id,Long userId,Long teamId);

	Optional<Project> findByIdAndUserIdAndTeamId(Long id, Long userId, Long teamId);

	Optional<Project> findByIdAndTeamId(Long projectId, Long teamId);

	
}
